<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Access Terminal</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Segoe+UI:wght@400;600;700&display=swap');

        :root {
            --neon-green: #00ff9f;
            --neon-red: #ff0055;
            --bg-color: #000000;
            --glass-bg: rgba(0, 10, 0, 0.6);
            --glass-border: rgba(0, 255, 159, 0.3);
            --glass-shadow: rgba(0, 255, 159, 0.2);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--neon-green);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        /* Matrix Background */
        canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
        }

        /* Glassmorphism Container */
        .container {
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            background: var(--glass-bg);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            box-shadow: 0 0 25px var(--glass-shadow);
            width: 90%;
            max-width: 420px;
            padding: 2.5rem;
            position: relative;
            z-index: 10;
            max-height: 90vh;
            overflow-x: hidden;
            overflow-y: auto;
            transition: all 0.3s ease;
            scrollbar-width: none; /* Hide scrollbar for Firefox */
            -ms-overflow-style: none; /* Hide scrollbar for IE/Edge */
        }

        /* Hide Scrollbar for Chrome, Safari, and Opera */
        .container::-webkit-scrollbar {
            display: none;
        }

        h2 {
            text-align: center;
            margin-bottom: 1.5rem;
            font-weight: 600;
            letter-spacing: 2px;
            text-transform: uppercase;
            text-shadow: 0 0 10px var(--glass-shadow);
        }

        .screen {
            display: none;
            flex-direction: column;
            animation: fadeIn 0.4s ease forwards;
        }

        .screen.active {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Inputs */
        input {
            width: 100%;
            padding: 12px 15px;
            margin-bottom: 1.2rem;
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid var(--glass-border);
            border-radius: 8px;
            color: #ffffff;
            font-size: 1rem;
            outline: none;
            transition: all 0.3s ease;
        }

        input::placeholder {
            color: rgba(0, 255, 159, 0.4);
        }

        input:focus {
            border-color: var(--neon-green);
            box-shadow: 0 0 12px var(--glass-shadow);
        }

        /* Search input specific tweak to reduce bottom spacing before the list */
        #search-input {
            margin-bottom: 1rem;
        }

        /* Buttons */
        button {
            background: transparent;
            color: var(--neon-green);
            border: 1px solid var(--neon-green);
            padding: 12px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            letter-spacing: 1px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
        }

        button:hover {
            background: var(--neon-green);
            color: var(--bg-color);
            box-shadow: 0 0 15px var(--glass-shadow);
        }

        /* Login Specifics */
        #login-msg {
            text-align: center;
            margin-top: 1rem;
            color: var(--neon-red);
            font-weight: 600;
            min-height: 24px;
            text-shadow: 0 0 8px rgba(255, 0, 85, 0.4);
        }

        /* Dashboard Specifics */
        .header-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }

        .header-row h2 {
            margin-bottom: 0;
            font-size: 1.3rem;
            text-align: left;
        }

        .btn-circular {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.5rem;
            padding: 0;
        }

        ul#app-list {
            list-style-type: none;
        }

        ul#app-list li {
            padding: 1rem 1.2rem;
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid var(--glass-border);
            border-radius: 8px;
            margin-bottom: 0.8rem;
            cursor: pointer;
            font-weight: 600;
            letter-spacing: 1px;
            transition: all 0.3s ease;
        }

        ul#app-list li:hover {
            border-color: var(--neon-green);
            box-shadow: 0 0 10px var(--glass-shadow);
            transform: translateX(5px);
        }

        /* Details Screen Specifics */
        .data-box {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem;
            border: 1px solid var(--glass-border);
            border-radius: 8px;
            margin-bottom: 1rem;
            background: rgba(0, 0, 0, 0.4);
            transition: border-color 0.3s;
        }

        .data-box:hover {
            border-color: var(--neon-green);
        }

        .data-content {
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .data-label {
            font-size: 0.75rem;
            color: rgba(0, 255, 159, 0.7);
            text-transform: uppercase;
            margin-bottom: 0.3rem;
        }

        .data-value {
            color: #ffffff;
            font-size: 1rem;
            font-family: monospace;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .copy-icon {
            cursor: pointer;
            width: 24px;
            height: 24px;
            stroke: var(--neon-green);
            fill: none;
            transition: all 0.2s ease;
            flex-shrink: 0;
            margin-left: 1rem;
        }

        .copy-icon:hover {
            stroke: #ffffff;
            filter: drop-shadow(0 0 5px var(--neon-green));
            transform: scale(1.1);
        }

        .copy-icon:active {
            transform: scale(0.9);
        }

        .action-row {
            display: flex;
            gap: 1rem;
            margin-top: 1rem;
        }

        .action-row button {
            flex: 1;
        }

    </style>
</head>
<body>

    <canvas id="matrix-bg"></canvas>

    <div class="container">
        
        <div id="login-screen" class="screen active">
            <h2>ACCESS TERMINAL</h2>
            <input type="password" id="password-input" placeholder="Enter Password" autocomplete="off">
            <button id="login-btn">ENTER</button>
            <div id="login-msg"></div>
        </div>

        <div id="dashboard-screen" class="screen">
            <div class="header-row">
                <h2>ACCESS GRANTED</h2>
                <button id="btn-show-add" class="btn-circular">+</button>
            </div>
            <input type="text" id="search-input" placeholder="Search app..." autocomplete="off">
            <ul id="app-list">
                </ul>
        </div>

        <div id="add-screen" class="screen">
            <h2>ADD RECORD</h2>
            <input type="text" id="add-name" placeholder="App Name" autocomplete="off">
            <input type="text" id="add-user" placeholder="Username" autocomplete="off">
            <input type="text" id="add-pass" placeholder="Password" autocomplete="off">
            <input type="text" id="add-num" placeholder="Number" autocomplete="off">
            <input type="text" id="add-secret" placeholder="2FA Secret Code" autocomplete="off">
            <div class="action-row">
                <button id="btn-cancel-add">BACK</button>
                <button id="btn-save">SAVE</button>
            </div>
        </div>

        <div id="details-screen" class="screen">
            <h2 id="detail-title">APP NAME</h2>
            
            <div class="data-box">
                <div class="data-content">
                    <span class="data-label">Username</span>
                    <span class="data-value" id="val-user"></span>
                </div>
                <svg class="copy-icon" onclick="copyData('val-user')" viewBox="0 0 24 24" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
                    <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
                </svg>
            </div>

            <div class="data-box">
                <div class="data-content">
                    <span class="data-label">Password</span>
                    <span class="data-value" id="val-pass"></span>
                </div>
                <svg class="copy-icon" onclick="copyData('val-pass')" viewBox="0 0 24 24" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
                    <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
                </svg>
            </div>

            <div class="data-box">
                <div class="data-content">
                    <span class="data-label">Number</span>
                    <span class="data-value" id="val-num"></span>
                </div>
                <svg class="copy-icon" onclick="copyData('val-num')" viewBox="0 0 24 24" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
                    <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
                </svg>
            </div>

            <div class="data-box">
                <div class="data-content">
                    <span class="data-label">2FA Secret Code</span>
                    <span class="data-value" id="val-secret"></span>
                </div>
                <svg class="copy-icon" onclick="copyData('val-secret')" viewBox="0 0 24 24" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
                    <path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>
                </svg>
            </div>

            <button id="btn-back-details" style="margin-top: 1rem;">BACK</button>
        </div>

    </div>

    <script>
        // --- CONTINUOUS MATRIX BACKGROUND ANIMATION ---
        const canvas = document.getElementById('matrix-bg');
        const ctx = canvas.getContext('2d');

        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;
        const fontSize = 16;
        let columns = Math.floor(width / fontSize);
        const characters = '01'.split('');
        let drops = [];

        for (let x = 0; x < columns; x++) {
            drops[x] = 1;
        }

        let lastTime = 0;
        const fpsInterval = 35;

        function drawMatrix(timestamp) {
            requestAnimationFrame(drawMatrix);
            
            const elapsed = timestamp - lastTime;
            if (elapsed < fpsInterval) return;
            lastTime = timestamp - (elapsed % fpsInterval);

            ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            ctx.fillRect(0, 0, width, height);
            
            ctx.fillStyle = '#00ff9f';
            ctx.font = fontSize + 'px monospace';
            
            for (let i = 0; i < drops.length; i++) {
                const text = characters[Math.floor(Math.random() * characters.length)];
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                
                if (drops[i] * fontSize > height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }

        window.addEventListener('resize', () => {
            if (window.innerWidth === width) return; 
            
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            
            const newColumns = Math.floor(width / fontSize);
            for (let x = columns; x < newColumns; x++) {
                drops[x] = 1;
            }
            columns = newColumns;
        });

        requestAnimationFrame(drawMatrix);


        // --- APPLICATION LOGIC ---
        
        const screens = {
            login: document.getElementById('login-screen'),
            dashboard: document.getElementById('dashboard-screen'),
            add: document.getElementById('add-screen'),
            details: document.getElementById('details-screen')
        };

        function switchScreen(screenName) {
            Object.values(screens).forEach(screen => screen.classList.remove('active'));
            screens[screenName].classList.add('active');
        }

        // 1. LOGIN SYSTEM -> Directly to Dashboard
        document.getElementById('login-btn').addEventListener('click', () => {
            const passInput = document.getElementById('password-input').value;
            const msgBox = document.getElementById('login-msg');
            
            if (passInput === '1234') {
                msgBox.textContent = '';
                document.getElementById('password-input').value = '';
                document.getElementById('search-input').value = ''; 
                renderAppList();
                switchScreen('dashboard');
            } else {
                msgBox.textContent = 'ACCESS DENIED';
                setTimeout(() => msgBox.textContent = '', 2000);
            }
        });

        document.getElementById('password-input').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') document.getElementById('login-btn').click();
        });


        // 2 & 4. DASHBOARD, SEARCH & DATA PERSISTENCE
        function getApps() {
            const apps = localStorage.getItem('terminal_apps');
            return apps ? JSON.parse(apps) : [];
        }

        function renderAppList() {
            const appList = document.getElementById('app-list');
            appList.innerHTML = '';
            const apps = getApps();
            const query = document.getElementById('search-input').value.toLowerCase().trim();

            const filteredApps = apps.filter(app => 
                (app.name || '').toLowerCase().includes(query)
            );

            if (apps.length === 0) {
                appList.innerHTML = '<li style="text-align: center; border-color: transparent; color: rgba(0,255,159,0.5);">No records found</li>';
                return;
            }

            if (filteredApps.length === 0) {
                appList.innerHTML = '<li style="text-align: center; border-color: transparent; color: rgba(0,255,159,0.5);">No results</li>';
                return;
            }

            filteredApps.forEach((app) => {
                const trueIndex = apps.findIndex(a => a === app);
                const li = document.createElement('li');
                li.textContent = app.name || 'Unnamed App';
                li.onclick = () => openDetails(trueIndex);
                appList.appendChild(li);
            });
        }

        // Search filtering logic
        document.getElementById('search-input').addEventListener('input', renderAppList);

        // 3. ADD APP SYSTEM
        document.getElementById('btn-show-add').addEventListener('click', () => {
            switchScreen('add');
        });

        document.getElementById('btn-cancel-add').addEventListener('click', () => {
            clearAddFields();
            switchScreen('dashboard');
        });

        document.getElementById('btn-save').addEventListener('click', () => {
            const name = document.getElementById('add-name').value.trim();
            const user = document.getElementById('add-user').value.trim();
            const pass = document.getElementById('add-pass').value.trim();
            const num = document.getElementById('add-num').value.trim();
            const secret = document.getElementById('add-secret').value.trim();

            if (!name) {
                alert('App Name is required');
                return;
            }

            const apps = getApps();
            apps.push({ name, user, pass, num, secret });
            localStorage.setItem('terminal_apps', JSON.stringify(apps));
            
            clearAddFields();
            document.getElementById('search-input').value = ''; 
            renderAppList();
            switchScreen('dashboard');
        });

        function clearAddFields() {
            document.getElementById('add-name').value = '';
            document.getElementById('add-user').value = '';
            document.getElementById('add-pass').value = '';
            document.getElementById('add-num').value = '';
            document.getElementById('add-secret').value = '';
        }

        // 5. DETAILS VIEW
        function openDetails(index) {
            const apps = getApps();
            const app = apps[index];
            if (!app) return;

            document.getElementById('detail-title').textContent = app.name;
            document.getElementById('val-user').textContent = app.user || 'N/A';
            document.getElementById('val-pass').textContent = app.pass || 'N/A';
            document.getElementById('val-num').textContent = app.num || 'N/A';
            document.getElementById('val-secret').textContent = app.secret || 'N/A';

            switchScreen('details');
        }

        // 6. COPY FUNCTION
        function copyData(elementId) {
            const textToCopy = document.getElementById(elementId).textContent;
            if (textToCopy === 'N/A' || !textToCopy) return;

            navigator.clipboard.writeText(textToCopy).catch(err => {
                console.error('Could not copy text: ', err);
            });
        }

        // 7. BACK BUTTON (Details to Main)
        document.getElementById('btn-back-details').addEventListener('click', () => {
            switchScreen('dashboard');
        });
    </script>
</body>
</html># Kamz-vault
