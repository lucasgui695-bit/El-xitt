<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>FFH4X Floating Panel</title>
    <style>
        :root {
            --primary: #ff0040;
            --bg: rgba(20, 20, 20, 0.95);
            --card: #1a1a1a;
            --text: #ffffff;
        }

        body {
            margin: 0;
            height: 100vh;
            background: #222; /* Cor de fundo apenas para teste */
            overflow: hidden;
            font-family: 'Segoe UI', sans-serif;
        }

        /* Container do Painel Flutuante */
        #floating-panel {
            position: absolute;
            top: 50px;
            left: 20px;
            width: 260px;
            background: var(--bg);
            border: 2px solid var(--primary);
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 0 25px rgba(255, 0, 64, 0.5);
            z-index: 9999;
            touch-action: none; /* Necessário para o drag no mobile */
        }

        /* Barra de Arrastar */
        .drag-handle {
            background: var(--primary);
            margin: -15px -15px 15px -15px;
            padding: 8px;
            text-align: center;
            font-weight: bold;
            font-size: 14px;
            border-radius: 10px 10px 0 0;
            cursor: move;
            color: white;
            text-shadow: 1px 1px 2px #000;
        }

        .section { margin-bottom: 12px; }
        
        label {
            display: block;
            font-size: 11px;
            color: #aaa;
            margin-bottom: 4px;
            text-transform: uppercase;
        }

        input[type=range] { width: 100%; accent-color: var(--primary); }

        .btn-group { display: flex; gap: 4px; }

        .btn-opt {
            background: #333;
            border: 1px solid #444;
            color: white;
            padding: 6px;
            border-radius: 4px;
            flex: 1;
            font-size: 9px;
            cursor: pointer;
        }

        .btn-opt.active { background: var(--primary); border-color: #fff; }

        #injectBtn {
            width: 100%;
            padding: 10px;
            background: var(--primary);
            border: none;
            color: white;
            font-weight: bold;
            border-radius: 5px;
            margin-top: 10px;
            font-size: 12px;
        }

        /* Ícone de Minimizar */
        #minimize-icon {
            position: fixed;
            top: 100px;
            left: 0;
            width: 50px;
            height: 50px;
            background: var(--primary);
            border-radius: 0 50% 50% 0;
            display: none;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            color: white;
            box-shadow: 2px 2px 10px rgba(0,0,0,0.5);
            z-index: 10000;
        }
    </style>
</head>
<body>

    <div id="minimize-icon" onclick="togglePanel()">FF</div>

    <div id="floating-panel">
        <div class="drag-handle" id="header">FFH4X MOD MENU</div>
        
        <div class="section">
            <label>Aimbot Sensitivity</label>
            <input type="range" min="0" max="100" value="70" oninput="resetInject()">
        </div>

        <div class="section">
            <label>Aim Location</label>
            <div class="btn-group">
                <button class="btn-opt" onclick="setAim('Cabeça', this)">CABEÇA</button>
                <button class="btn-opt" onclick="setAim('Pescoço', this)">PESCOÇO</button>
                <button class="btn-opt" onclick="setAim('Peito', this)">PEITO</button>
            </div>
        </div>

        <div class="section">
            <label>Auto Headshot %</label>
            <input type="range" min="0" max="100" value="100" oninput="resetInject()">
        </div>

        <button id="injectBtn" onclick="startInjection()">INJETAR</button>
        
        <button onclick="togglePanel()" style="width:100%; background:none; border:none; color:#777; font-size:10px; margin-top:10px;">ESCONDER PAINEL</button>
    </div>

    <script>
        const panel = document.getElementById('floating-panel');
        const icon = document.getElementById('minimize-icon');
        const injectBtn = document.getElementById('injectBtn');

        // Lógica de Arrastar (Mobile e PC)
        let isDragging = false;
        let currentX;
        let currentY;
        let initialX;
        let initialY;
        let xOffset = 0;
        let yOffset = 0;

        const header = document.getElementById('header');

        header.addEventListener("mousedown", dragStart);
        header.addEventListener("touchstart", dragStart);
        window.addEventListener("mousemove", drag);
        window.addEventListener("touchmove", drag);
        window.addEventListener("mouseup", dragEnd);
        window.addEventListener("touchend", dragEnd);

        function dragStart(e) {
            initialX = (e.type === "touchstart" ? e.touches[0].clientX : e.clientX) - xOffset;
            initialY = (e.type === "touchstart" ? e.touches[0].clientY : e.clientY) - yOffset;
            if (e.target === header) isDragging = true;
        }

        function drag(e) {
            if (isDragging) {
                e.preventDefault();
                currentX = (e.type === "touchmove" ? e.touches[0].clientX : e.clientX) - initialX;
                currentY = (e.type === "touchmove" ? e.touches[0].clientY : e.clientY) - initialY;
                xOffset = currentX;
                yOffset = currentY;
                panel.style.transform = `translate3d(${currentX}px, ${currentY}px, 0)`;
            }
        }

        function dragEnd() { isDragging = false; }

        // Funções do Painel
        function setAim(type, el) {
            document.querySelectorAll('.btn-opt').forEach(b => b.classList.remove('active'));
            el.classList.add('active');
            resetInject();
        }

        function resetInject() {
            injectBtn.innerText = "INJETAR";
            injectBtn.style.background = "#ff0040";
        }

        function startInjection() {
            injectBtn.innerText = "INJETANDO CÓDIGOS...";
            setTimeout(() => {
                injectBtn.innerText = "CÓDIGOS INJETADOS COM SUCESSO";
                injectBtn.style.background = "#28a745";
            }, 5000);
        }

        function togglePanel() {
            if (panel.style.display === "none") {
                panel.style.display = "block";
                icon.style.display = "none";
            } else {
                panel.style.display = "none";
                icon.style.display = "flex";
            }
        }
    </script>
</body>
</html>
