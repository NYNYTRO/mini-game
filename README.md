<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MASTER_STATION_v4.0</title>
    <style>
        body { 
            background: #245edb linear-gradient(180deg, #5ba3ff 0%, #245edb 50%, #003399 100%);
            margin: 0; overflow: hidden; font-family: 'Tahoma', sans-serif; height: 100vh;
        }

        /* ICONS DREAPTA */
        .desktop-right {
            position: absolute; right: 40px; top: 40px;
            display: flex; flex-direction: column; gap: 45px; z-index: 100;
        }
        .icon {
            width: 100px; text-align: center; cursor: pointer; position: relative;
            filter: drop-shadow(4px 4px 5px rgba(0,0,0,0.8)); transition: 0.1s;
        }
        .icon:active { transform: scale(0.9); }
        .img-svg { width: 55px; height: 55px; margin: 0 auto 8px; display: block; }
        .icon span { color: white; font-size: 13px; font-weight: bold; text-shadow: 2px 2px #000; }

        /* ALERTA JOCLAN */
        .alert-mini {
            position: absolute; bottom: 30px; right: 15px; width: 28px; height: 28px;
            background: #ff0000; border: 2px solid white; border-radius: 50%;
            color: white; font-weight: bold; font-size: 20px; line-height: 28px;
            animation: pulse-danger 0.3s infinite; text-align: center; box-shadow: 0 0 15px red;
        }

        /* TASKBAR & START */
        #taskbar {
            position: fixed; bottom: 0; width: 100%; height: 35px;
            background: linear-gradient(to bottom, #245edb 0%, #3f8cf3 10%, #245edb 100%);
            display: flex; align-items: center; border-top: 1px solid #1a3a8a; z-index: 1000;
        }
        .start-btn {
            background: linear-gradient(to bottom, #388e3c, #4caf50);
            width: 120px; height: 100%; border: none; border-radius: 0 15px 0 0;
            color: white; font-weight: bold; font-style: italic; font-size: 19px;
            text-shadow: 1px 1px 2px #000; cursor: pointer;
        }

        /* START MENU */
        #start-menu {
            position: fixed; bottom: 35px; left: 0; width: 280px; background: #fff;
            border: 2px solid #245edb; display: none; z-index: 999; box-shadow: 5px -5px 30px #000;
        }
        .menu-head { background: linear-gradient(to right, #1a3a8a, #4287f5); color: white; padding: 12px; font-weight: bold; }
        .menu-item { padding: 12px; border-bottom: 1px solid #eee; color: #333; cursor: pointer; }
        .menu-item:hover { background: #3f8cf3; color: white; }

        /* ERROR WINDOWS */
        .error-window {
            width: 380px; background: #ece9d8; border: 2px solid #0054e3;
            position: absolute; z-index: 1100; box-shadow: 20px 20px 80px rgba(0,0,0,0.9);
            border-radius: 8px 8px 0 0;
        }
        .error-header { background: linear-gradient(to right, #0058e6, #4593ff); color: white; padding: 6px 12px; font-weight: bold; }

        @keyframes pulse-danger { 0% { transform: scale(1); } 50% { transform: scale(1.4); background: #000; } 100% { transform: scale(1); } }
        .hacked { animation: shake 0.05s infinite; filter: invert(1) contrast(2) hue-rotate(180deg) !important; }
        @keyframes shake { 0% { transform: translate(5px, 5px); } 50% { transform: translate(-5px, -5px); } }
    </style>
</head>
<body id="main">

    <div class="desktop-right">
        <!-- MASTER COMPUTER -->
        <div class="icon" onclick="toggleStart()">
            <svg class="img-svg" viewBox="0 0 48 48"><rect x="4" y="6" width="40" height="28" fill="#ccc" stroke="black"/><rect x="8" y="10" width="32" height="20" fill="#0058e6"/><rect x="16" y="34" width="16" height="4" fill="#666"/><rect x="10" y="38" width="28" height="4" fill="#333"/></svg>
            <span>Master_PC</span>
        </div>
        
        <!-- FOLDERUL JOCLAN (VIRUSUL) -->
        <div class="icon" onclick="START_BOOM()">
            <svg class="img-svg" viewBox="0 0 48 48"><path d="M4,10 L18,10 L22,14 L44,14 L44,40 L4,40 Z" fill="#ffd700" stroke="#b8860b"/></svg>
            <div class="alert-mini">!</div>
            <span style="color: #ff0; font-size: 14px;">JOCLAN_v1.exe</span>
        </div>

        <!-- RECYCLE BIN -->
        <div class="icon">
            <svg class="img-svg" viewBox="0 0 48 48"><rect x="12" y="14" width="24" height="28" fill="#ccc" stroke="black"/><rect x="10" y="10" width="28" height="4" fill="#999"/></svg>
            <span>Trash_Bin</span>
        </div>
    </div>

    <!-- TASKBAR -->
    <div id="taskbar">
        <button class="start-btn" onclick="toggleStart()">start</button>
        <div style="margin-left: 20px; color: white; font-weight: bold;">SYSTEM_MASTER_STATION</div>
    </div>

    <!-- START MENU -->
    <div id="start-menu">
        <div class="menu-head">CHIEF_ADMINISTRATOR</div>
        <div class="menu-item" onclick="alert('Accessing Mainframe...')">Control_Panel</div>
        <div class="menu-item" onclick="alert('Secured Files Locked.')">My_Vault</div>
        <div class="menu-item" style="color:red; font-weight:bold;" onclick="location.reload()">SHUTDOWN_STATION</div>
    </div>

    <script>
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        
        function playErrorSound() {
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(120 + Math.random()*300, audioCtx.currentTime);
            gain.gain.setValueAtTime(0.05, audioCtx.currentTime);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + 0.1);
        }

        function toggleStart() {
            const m = document.getElementById('start-menu');
            m.style.display = (m.style.display === 'block') ? 'none' : 'block';
        }

        function START_BOOM() {
            document.body.classList.add('hacked');
            let count = 0;
            const interval = setInterval(() => {
                count++;
                playErrorSound();
                const win = document.createElement('div');
                win.className = 'error-window';
                win.style.left = Math.random() * (window.innerWidth - 300) + 'px';
                win.style.top = Math.random() * (window.innerHeight - 200) + 'px';
                win.innerHTML = `
                    <div class="error-header">CRITICAL_SYSTEM_INFECTED</div>
                    <div style="padding:25px; background:white; color:black; font-weight:bold;">
                        <span style="color:red; font-size:24px;">BY JOCLAN</span><br><br>
                        Administrator privileges revoked.<br>
                        Wiping local drives...
                    </div>
                `;
                document.body.appendChild(win);
                
                if(count > 60) {
                    clearInterval(interval);
                    document.body.innerHTML = '<div style="background:#0000aa; color:#fff; height:100vh; padding:50px; font-family:monospace; font-size:22px;">A critical error has occurred: MASTER_STATION_FAILURE<br><br>INFECTED_BY: JOCLAN_v1.exe<br>RESTART_DISABLED</div>';
                }
            }, 100);
        }

        window.onclick = (e) => {
            if (!e.target.matches('.start-btn')) document.getElementById('start-menu').style.display = 'none';
        }
    </script>
</body>
</html>
