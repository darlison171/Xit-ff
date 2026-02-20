<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PAINEL FFH4X PRO - REBORN</title>
    <style>
        /* Estilização Geral */
        * { box-sizing: border-box; font-family: 'Segoe UI', sans-serif; user-select: none; }
        body { 
            background-color: #080808; 
            display: flex; 
            justify-content: center; 
            align-items: center; 
            height: 100vh; 
            margin: 0; 
            color: white; 
        }

        /* Container do Painel */
        .panel {
            width: 400px;
            background: #000;
            border: 1px solid #bc00ff;
            border-radius: 10px;
            box-shadow: 0 0 25px rgba(188, 0, 255, 0.4);
            overflow: hidden;
        }

        /* Barra de Título */
        .header {
            background: #bc00ff;
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            font-weight: bold;
            font-size: 13px;
        }

        .main { display: flex; height: 320px; }

        /* Barra Lateral (Sidebar) */
        .sidebar {
            width: 70px;
            background: #0a0a0a;
            border-right: 1px solid #1a1a1a;
            display: flex;
            flex-direction: column;
            padding: 10px;
            gap: 12px;
        }

        .icon-tab {
            width: 50px; height: 50px;
            background: #151515;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            border: 1px solid #222;
            font-size: 20px;
            transition: 0.3s;
        }
        .icon-tab.active { border-color: #bc00ff; background: rgba(188, 0, 255, 0.1); box-shadow: 0 0 10px rgba(188, 0, 255, 0.2); }

        /* Conteúdo Principal */
        .content { flex: 1; padding: 20px; overflow-y: auto; }

        .section-title { color: #bc00ff; font-size: 11px; text-transform: uppercase; margin-bottom: 15px; letter-spacing: 1px; font-weight: bold; }

        .option {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            font-size: 13px;
        }

        /* Botão Checkbox Personalizado */
        .box { width: 40px; height: 20px; background: #222; border-radius: 20px; position: relative; cursor: pointer; transition: 0.4s; border: 1px solid #333; }
        .box.active { background: #bc00ff; }
        .box::before { content: ""; position: absolute; width: 14px; height: 14px; background: white; border-radius: 50%; top: 2px; left: 3px; transition: 0.4s; }
        .box.active::before { transform: translateX(18px); }

        /* Sliders */
        .slider-container { margin-bottom: 20px; }
        .slider-info { display: flex; justify-content: space-between; font-size: 12px; margin-bottom: 8px; color: #bbb; }
        input[type=range] { width: 100%; accent-color: #bc00ff; cursor: pointer; background: #222; border-radius: 5px; height: 6px; appearance: none; }

        /* Rodapé e Botões */
        .footer {
            background: #0a0a0a;
            padding: 15px;
            border-top: 1px solid #1a1a1a;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        button {
            width: 100%;
            background: #bc00ff;
            border: none;
            color: white;
            padding: 10px;
            border-radius: 5px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
        }
        button:active { transform: scale(0.98); opacity: 0.8; }
        
        #status-text { text-align: center; font-size: 10px; color: #555; margin: 0; }

        /* Barra de Progresso Simulada */
        .progress-bar { width: 100%; height: 4px; background: #222; border-radius: 2px; display: none; margin-top: 5px; }
        .progress-fill { width: 0%; height: 100%; background: #bc00ff; border-radius: 2px; transition: 0.1s; }
    </style>
</head>
<body>

<div class="panel">
    <div class="header">
        <span>PAINEL FFH4X - MODO WEB</span>
        <span onclick="location.reload()" style="cursor:pointer">✕</span>
    </div>

    <div class="main">
        <div class="sidebar">
            <div class="icon-tab active">🎯</div>
            <div class="icon-tab">👁</div>
            <div class="icon-tab">⚙</div>
        </div>

        <div class="content">
            <div class="section-title">Aimbot & Accuracy</div>
            
            <div class="option">
                <span>Ativar Aimbot</span>
                <div class="box" onclick="this.classList.toggle('active')"></div>
            </div>

            <div class="slider-container">
                <div class="slider-info">
                    <span>Smooth (Suavidade)</span>
                    <span id="smooth-val">50%</span>
                </div>
                <input type="range" min="1" max="100" value="50" oninput="document.getElementById('smooth-val').innerText = this.value + '%'">
            </div>

            <div class="section-title">Visual & ESP</div>
            
            <div class="option">
                <span>ESP Line / Box</span>
                <div class="box active" onclick="this.classList.toggle('active')"></div>
            </div>

            <div class="slider-container">
                <div class="slider-info">
                    <span>FOV Size</span>
                    <span id="fov-val">120.0</span>
                </div>
                <input type="range" min="30" max="300" value="120" oninput="document.getElementById('fov-val').innerText = this.value + '.0'">
            </div>
        </div>
    </div>

    <div class="footer">
        <button id="main-btn" onclick="startSim()">ATIVAR NO NAVEGADOR</button>
        <div class="progress-bar" id="bar-bg"><div class="progress-fill" id="fill"></div></div>
        <p id="status-text">Status: Aguardando entrada...</p>
    </div>
</div>

<script>
    function startSim() {
        const btn = document.getElementById('main-btn');
        const barBg = document.getElementById('bar-bg');
        const fill = document.getElementById('fill');
        const status = document.getElementById('status-text');

        btn.style.display = 'none';
        barBg.style.display = 'block';
        
        let progress = 0;
        const interval = setInterval(() => {
            if (progress >= 100) {
                clearInterval(interval);
                status.innerText = "SIMULAÇÃO CONCLUÍDA! (Visual Ativo)";
                status.style.color = "#bc00ff";
                alert("Painel visual ativado com sucesso no seu navegador!");
            } else {
                progress++;
                fill.style.width = progress + "%";
                status.innerText = "Processando dados... " + progress + "%";
            }
        }, 40);
    }
</script>

</body>
</html>
