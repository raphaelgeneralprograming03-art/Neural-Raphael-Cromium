
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neural-Raphael-Cromium | Direct Web Engine Browser</title>
    <style>
        :root {
            --bg-color: #0b0c0e;
            --toolbar-color: #16181d;
            --accent-color: #ff2a2a;
            --accent-glow: rgba(255, 42, 42, 0.4);
            --dark-ring: #050507;
            --text-color: #f1f1f5;
            --text-dim: #8e929b;
            --tab-inactive: #20232b;
            --tab-hover: #2a2d37;
            --border-color: #2d313c;
            --success-color: #00e676;
        }

        * { box-sizing: border-box; user-select: none; margin: 0; padding: 0; }

        body, html {
            height: 100%; width: 100%;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background: var(--bg-color); color: var(--text-color);
            overflow: hidden;
        }

        /* --- CHROMIUM RED/BLACK LOGO --- */
        .chromium-logo {
            width: 28px; height: 28px;
            position: relative; border-radius: 50%;
            background: var(--dark-ring); display: inline-block;
            box-shadow: 0 0 10px var(--accent-glow);
            flex-shrink: 0; cursor: pointer;
        }

        .chromium-logo .center-circle {
            position: absolute; top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            width: 10px; height: 10px;
            background: #ff0000; border-radius: 50%;
            z-index: 4; box-shadow: 0 0 8px #ff0000;
        }

        .chromium-logo .segment {
            position: absolute; width: 100%; height: 100%;
            border-radius: 50%;
            clip-path: polygon(50% 50%, 0 0, 100% 0);
        }

        .chromium-logo .seg1 { background: #1a1a1a; transform: rotate(0deg); }
        .chromium-logo .seg2 { background: #111111; transform: rotate(120deg); }
        .chromium-logo .seg3 { background: #080808; transform: rotate(240deg); }

        .chromium-logo .outer-ring {
            position: absolute; inset: 0;
            border-radius: 50%; border: 2px solid #222; z-index: 3;
        }

        /* BROWSER UI */
        #browser-ui { display: flex; flex-direction: column; height: 100vh; width: 100vw; }

        #tabs-bar {
            display: flex; align-items: center; background: #0d0e11;
            padding: 6px 6px 0 6px; gap: 4px; overflow-x: auto;
            border-bottom: 1px solid var(--border-color);
        }

        .tab {
            background: var(--tab-inactive); padding: 7px 14px;
            border-radius: 8px 8px 0 0; font-size: 12px; cursor: pointer;
            min-width: 140px; max-width: 220px; display: flex;
            justify-content: space-between; align-items: center; gap: 8px;
            color: var(--text-dim); border: 1px solid transparent; border-bottom: none;
        }

        .tab.active {
            background: var(--toolbar-color); color: var(--text-color);
            border-color: var(--border-color); border-top: 2px solid var(--accent-color);
        }

        .tab .tab-title { white-space: nowrap; overflow: hidden; text-overflow: ellipsis; flex-grow: 1; }

        header {
            background: var(--toolbar-color); padding: 8px 12px;
            display: flex; align-items: center; gap: 10px;
            border-bottom: 1px solid var(--border-color);
        }

        .nav-buttons { display: flex; gap: 4px; }

        .btn-icon {
            background: #20232c; border: 1px solid var(--border-color);
            color: var(--text-color); width: 32px; height: 32px;
            border-radius: 6px; cursor: pointer; display: flex;
            align-items: center; justify-content: center; font-size: 14px;
        }

        .btn-icon:hover { background: var(--accent-color); border-color: var(--accent-color); }

        #address-container { flex-grow: 1; position: relative; display: flex; align-items: center; }

        #address-bar {
            width: 100%; background: #090a0c; border: 1px solid var(--border-color);
            padding: 8px 40px 8px 36px; border-radius: 20px; color: #fff;
            font-size: 13px; outline: none;
        }

        #address-bar:focus { border-color: var(--accent-color); box-shadow: 0 0 10px var(--accent-glow); }

        .protocol-icon { position: absolute; left: 12px; font-size: 12px; color: var(--success-color); }

        #viewport { flex-grow: 1; background: #111216; position: relative; width: 100%; height: 100%; }

        .frame-container { width: 100%; height: 100%; display: none; position: absolute; inset: 0; }
        .frame-container.active { display: block; }

        iframe { width: 100%; height: 100%; border: none; background: #fff; }

        /* OVERVIEW OVERLAY */
        #overview-overlay {
            position: fixed; inset: 0; background: rgba(5, 5, 8, 0.94);
            display: none; z-index: 1000; backdrop-filter: blur(15px);
            flex-direction: column;
        }
        #overview-overlay.active { display: flex; }

        .overview-header {
            padding: 16px 20px; display: flex; justify-content: space-between;
            align-items: center; border-bottom: 1px solid var(--border-color);
        }

        #overview-canvas { flex-grow: 1; width: 100%; height: 100%; cursor: pointer; }
    </style>
</head>
<body>

    <div id="browser-ui">
        <div id="tabs-bar">
            <button class="btn-icon" style="height:24px; width:24px;" onclick="neuralBrowser.addTab()">+</button>
        </div>

        <header>
            <div class="chromium-logo" onclick="neuralBrowser.toggleOverview()" title="Overview Canvas">
                <div class="outer-ring"></div>
                <div class="center-circle"></div>
                <div class="segment seg1"></div>
                <div class="segment seg2"></div>
                <div class="segment seg3"></div>
            </div>

            <div class="nav-buttons">
                <button class="btn-icon" onclick="neuralBrowser.reload()">↻</button>
            </div>

            <div id="address-container">
                <span class="protocol-icon" id="protocol-status">🌐</span>
                <input type="text" id="address-bar" value="https://wikipedia.org" onkeydown="neuralBrowser.handleUrlKey(event)">
            </div>

            <div class="nav-buttons">
                <button class="btn-icon" onclick="neuralBrowser.toggleOverview()">🔳</button>
            </div>
        </header>

        <main id="viewport"></main>
    </div>

    <!-- OVERVIEW CANVAS OVERLAY -->
    <div id="overview-overlay">
        <div class="overview-header">
            <div style="font-weight:bold; display:flex; align-items:center; gap:10px;">
                <div class="chromium-logo" style="transform:scale(0.8)">
                    <div class="outer-ring"></div>
                    <div class="center-circle"></div>
                    <div class="segment seg1"></div>
                    <div class="segment seg2"></div>
                    <div class="segment seg3"></div>
                </div>
                Visão Geral das Guias Conectadas
            </div>
            <button class="btn-icon" onclick="neuralBrowser.toggleOverview()">✕</button>
        </div>
        <canvas id="overview-canvas"></canvas>
    </div>

    <script>
        /**
         * DIRECT WEB ENGINE - NEURAL-RAPHAEL-CROMIUM
         * Conexão direta com algoritmos e endpoints da Web Global
         */
        class DirectWebBrowser {
            constructor() {
                this.tabs = [];
                this.activeTabId = null;
                this.nextTabId = 1;
                // Engine de proxy reverso e Fetch direto para contornar headers X-Frame
                this.webDirectProxy = "https://api.allorigins.win/raw?url=";
                
                this.canvas = document.getElementById('overview-canvas');
                this.ctx = this.canvas.getContext('2d');
                
                this.init();
            }

            init() {
                this.addTab('Wikipedia', 'https://wikipedia.org');
                this.resizeCanvas();
                window.addEventListener('resize', () => this.resizeCanvas());
                this.canvas.addEventListener('click', (e) => this.handleCanvasClick(e));
                this.startCanvasLoop();
            }

            addTab(title = 'Nova Guia', url = 'https://wikipedia.org') {
                const id = this.nextTabId++;
                const tab = { id, title, url, color: this.getRandomColor() };
                this.tabs.push(tab);
                this.createViewportFrame(tab);
                this.switchTab(id);
                this.renderTabsUI();
            }

            switchTab(id) {
                this.activeTabId = id;
                const tab = this.tabs.find(t => t.id === id);
                document.querySelectorAll('.frame-container').forEach(f => f.classList.remove('active'));
                
                const activeFrame = document.getElementById(`frame-${id}`);
                if (activeFrame) activeFrame.classList.add('active');
                
                document.getElementById('address-bar').value = tab.url;
                this.renderTabsUI();
            }

            createViewportFrame(tab) {
                const viewport = document.getElementById('viewport');
                const container = document.createElement('div');
                container.id = `frame-${tab.id}`;
                container.className = 'frame-container';

                const iframe = document.createElement('iframe');
                // Motor de carregamento direto usando Web Direct Engine
                iframe.src = this.getDirectWebUrl(tab.url);
                container.appendChild(iframe);
                viewport.appendChild(container);
            }

            getDirectWebUrl(url) {
                if (url.startsWith('http://') || url.startsWith('https://')) {
                    return this.webDirectProxy + encodeURIComponent(url);
                }
                return this.webDirectProxy + encodeURIComponent('https://' + url);
            }

            handleUrlKey(e) {
                if (e.key === 'Enter') {
                    const inputUrl = e.target.value;
                    const tab = this.tabs.find(t => t.id === this.activeTabId);
                    tab.url = inputUrl;
                    tab.title = inputUrl.replace('https://', '').split('/')[0];
                    
                    const frame = document.getElementById(`frame-${tab.id}`);
                    if (frame) {
                        const iframe = frame.querySelector('iframe');
                        iframe.src = this.getDirectWebUrl(inputUrl);
                    }
                    this.renderTabsUI();
                }
            }

            reload() {
                const tab = this.tabs.find(t => t.id === this.activeTabId);
                const frame = document.getElementById(`frame-${tab.id}`);
                if (frame) {
                    const iframe = frame.querySelector('iframe');
                    iframe.src = this.getDirectWebUrl(tab.url);
                }
            }

            renderTabsUI() {
                const tabsBar = document.getElementById('tabs-bar');
                const oldTabs = tabsBar.querySelectorAll('.tab');
                oldTabs.forEach(t => t.remove());

                this.tabs.forEach(tab => {
                    const tabEl = document.createElement('div');
                    tabEl.className = `tab ${tab.id === this.activeTabId ? 'active' : ''}`;
                    tabEl.onclick = () => this.switchTab(tab.id);
                    tabEl.innerHTML = `<span class="tab-title">${tab.title}</span>`;
                    tabsBar.appendChild(tabEl);
                });
            }

            // --- CANVAS OVERVIEW 2D ENGINE ---
            toggleOverview() {
                const overlay = document.getElementById('overview-overlay');
                overlay.classList.toggle('active');
                if (overlay.classList.contains('active')) {
                    this.resizeCanvas();
                    this.drawOverview();
                }
            }

            resizeCanvas() {
                this.canvas.width = window.innerWidth;
                this.canvas.height = window.innerHeight - 60;
            }

            drawOverview() {
                this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
                const cardWidth = 260, cardHeight = 160, gap = 25;
                let x = 50, y = 40;

                this.tabs.forEach(tab => {
                    const isActive = tab.id === this.activeTabId;
                    this.ctx.save();
                    this.ctx.shadowBlur = isActive ? 15 : 5;
                    this.ctx.shadowColor = isActive ? "#ff2a2a" : "rgba(0,0,0,0.5)";
                    
                    this.ctx.fillStyle = "#16181d";
                    this.ctx.strokeStyle = isActive ? "#ff2a2a" : "#2d313c";
                    this.ctx.lineWidth = 2;
                    this.ctx.beginPath();
                    this.ctx.roundRect(x, y, cardWidth, cardHeight, 10);
                    this.ctx.fill();
                    this.ctx.stroke();

                    // Header
                    this.ctx.fillStyle = tab.color;
                    this.ctx.beginPath();
                    this.ctx.roundRect(x, y, cardWidth, 28, [10, 10, 0, 0]);
                    this.ctx.fill();

                    this.ctx.fillStyle = "#fff";
                    this.ctx.font = "bold 11px sans-serif";
                    this.ctx.fillText(tab.title, x + 10, y + 18);

                    tab.rect = { x, y, width: cardWidth, height: cardHeight };
                    this.ctx.restore();

                    x += cardWidth + gap;
                    if (x + cardWidth > this.canvas.width - 50) {
                        x = 50; y += cardHeight + gap;
                    }
                });
            }

            startCanvasLoop() {
                const loop = () => {
                    if (document.getElementById('overview-overlay').classList.contains('active')) {
                        this.drawOverview();
                    }
                    requestAnimationFrame(loop);
                };
                loop();
            }

            handleCanvasClick(e) {
                const rect = this.canvas.getBoundingClientRect();
                const mouseX = e.clientX - rect.left;
                const mouseY = e.clientY - rect.top;

                this.tabs.forEach(tab => {
                    if (tab.rect && mouseX >= tab.rect.x && mouseX <= tab.rect.x + tab.rect.width &&
                        mouseY >= tab.rect.y && mouseY <= tab.rect.y + tab.rect.height) {
                        this.switchTab(tab.id);
                        this.toggleOverview();
                    }
                });
            }

            getRandomColor() {
                const colors = ['#ff2a2a', '#00d4ff', '#7000ff', '#00e676', '#ffb300'];
                return colors[Math.floor(Math.random() * colors.length)];
            }
        }

        const neuralBrowser = new DirectWebBrowser();
    </script>
</body>
</html>
