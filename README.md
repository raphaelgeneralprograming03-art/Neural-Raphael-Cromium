
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neural-Raphael-Cromium | Next-Gen Engine Browser</title>
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

        * {
            box-sizing: border-box;
            user-select: none;
        }

        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background: var(--bg-color);
            color: var(--text-color);
            overflow: hidden;
        }

        /* --- LOGO ENGINE (CHROMIUM RED/BLACK REDESIGN) --- */
        .chromium-logo {
            width: 28px;
            height: 28px;
            position: relative;
            border-radius: 50%;
            background: var(--dark-ring);
            display: inline-block;
            box-shadow: 0 0 10px var(--accent-glow);
            flex-shrink: 0;
            cursor: pointer;
        }

        .chromium-logo .center-circle {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 10px;
            height: 10px;
            background: #ff0000;
            border-radius: 50%;
            z-index: 4;
            box-shadow: 0 0 8px #ff0000;
        }

        .chromium-logo .segment {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            clip-path: polygon(50% 50%, 0 0, 100% 0);
        }

        .chromium-logo .seg1 { background: #1a1a1a; transform: rotate(0deg); }
        .chromium-logo .seg2 { background: #111111; transform: rotate(120deg); }
        .chromium-logo .seg3 { background: #080808; transform: rotate(240deg); }

        .chromium-logo .outer-ring {
            position: absolute;
            inset: 0;
            border-radius: 50%;
            border: 2px solid #222;
            z-index: 3;
        }

        /* --- BROWSER UI LAYOUT --- */
        #browser-ui {
            display: flex;
            flex-direction: column;
            height: 100vh;
            width: 100vw;
        }

        /* TABS BAR */
        #tabs-bar {
            display: flex;
            align-items: center;
            background: #0d0e11;
            padding: 6px 6px 0 6px;
            gap: 4px;
            overflow-x: auto;
            border-bottom: 1px solid var(--border-color);
        }

        #tabs-bar::-webkit-scrollbar {
            height: 3px;
        }

        #tabs-bar::-webkit-scrollbar-thumb {
            background: var(--border-color);
        }

        .tab {
            background: var(--tab-inactive);
            padding: 7px 14px;
            border-radius: 8px 8px 0 0;
            font-size: 12px;
            cursor: pointer;
            min-width: 140px;
            max-width: 220px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 8px;
            transition: background 0.2s, color 0.2s;
            border: 1px solid transparent;
            border-bottom: none;
            color: var(--text-dim);
        }

        .tab:hover {
            background: var(--tab-hover);
            color: var(--text-color);
        }

        .tab.active {
            background: var(--toolbar-color);
            color: var(--text-color);
            border-color: var(--border-color);
            border-top: 2px solid var(--accent-color);
        }

        .tab .tab-title {
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            flex-grow: 1;
        }

        .tab .tab-close {
            border-radius: 50%;
            width: 16px;
            height: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            opacity: 0.6;
        }

        .tab .tab-close:hover {
            background: rgba(255,255,255,0.2);
            opacity: 1;
        }

        .add-tab-btn {
            background: transparent;
            border: none;
            color: var(--text-color);
            font-size: 16px;
            padding: 4px 10px;
            cursor: pointer;
            border-radius: 4px;
        }

        .add-tab-btn:hover {
            background: var(--tab-hover);
        }

        /* NAVBAR & TOOLBAR */
        header {
            background: var(--toolbar-color);
            padding: 8px 12px;
            display: flex;
            align-items: center;
            gap: 10px;
            border-bottom: 1px solid var(--border-color);
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        }

        .nav-buttons {
            display: flex;
            gap: 4px;
            align-items: center;
        }

        .btn-icon {
            background: #20232c;
            border: 1px solid var(--border-color);
            color: var(--text-color);
            width: 32px;
            height: 32px;
            border-radius: 6px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            transition: all 0.2s;
        }

        .btn-icon:hover {
            background: var(--accent-color);
            border-color: var(--accent-color);
            box-shadow: 0 0 8px var(--accent-glow);
        }

        #address-container {
            flex-grow: 1;
            position: relative;
            display: flex;
            align-items: center;
        }

        #address-bar {
            width: 100%;
            background: #090a0c;
            border: 1px solid var(--border-color);
            padding: 8px 40px 8px 36px;
            border-radius: 20px;
            color: #fff;
            font-size: 13px;
            outline: none;
            transition: all 0.25s;
        }

        #address-bar:focus {
            border-color: var(--accent-color);
            box-shadow: 0 0 10px var(--accent-glow);
        }

        .protocol-icon {
            position: absolute;
            left: 12px;
            font-size: 12px;
            color: var(--success-color);
        }

        .search-engine-selector {
            position: absolute;
            right: 10px;
            background: transparent;
            border: none;
            color: var(--text-dim);
            font-size: 11px;
            cursor: pointer;
            outline: none;
        }

        /* VIEWPORT AREA */
        #viewport {
            flex-grow: 1;
            background: #111216;
            position: relative;
            width: 100%;
            height: 100%;
        }

        .frame-container {
            width: 100%;
            height: 100%;
            display: none;
            position: absolute;
            inset: 0;
        }

        .frame-container.active {
            display: block;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
            background: #fff;
        }

        /* HOMEPAGE / NEW TAB */
        .new-tab-page {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100%;
            background: radial-gradient(circle at center, #1a1c23 0%, #0b0c0e 100%);
            color: var(--text-color);
            text-align: center;
            padding: 20px;
        }

        .new-tab-logo {
            transform: scale(2.5);
            margin-bottom: 40px;
        }

        .search-box-large {
            width: 100%;
            max-width: 600px;
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
        }

        .search-box-large input {
            flex-grow: 1;
            padding: 14px 20px;
            border-radius: 30px;
            border: 1px solid var(--border-color);
            background: #12141a;
            color: white;
            font-size: 16px;
            outline: none;
        }

        .search-box-large input:focus {
            border-color: var(--accent-color);
            box-shadow: 0 0 15px var(--accent-glow);
        }

        .shortcuts-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            max-width: 600px;
            width: 100%;
        }

        .shortcut-card {
            background: #181a20;
            padding: 15px;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            transition: transform 0.2s, border-color 0.2s;
        }

        .shortcut-card:hover {
            transform: translateY(-4px);
            border-color: var(--accent-color);
        }

        .shortcut-icon {
            width: 32px;
            height: 32px;
            background: var(--border-color);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        /* --- VISÃO GERAL / CANVAS OVERLAY --- */
        #overview-overlay {
            position: fixed;
            inset: 0;
            background: rgba(5, 5, 8, 0.92);
            display: none;
            z-index: 1000;
            backdrop-filter: blur(15px);
            flex-direction: column;
        }

        #overview-overlay.active {
            display: flex;
        }

        .overview-header {
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
        }

        .overview-title {
            font-size: 18px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        #overview-canvas {
            flex-grow: 1;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }

        /* --- DRAWER LATERAL (HISTÓRICO / EXTENSÕES / DOWNLOADS) --- */
        .side-drawer {
            position: fixed;
            right: -350px;
            top: 0;
            bottom: 0;
            width: 350px;
            background: #12141a;
            border-left: 1px solid var(--border-color);
            z-index: 500;
            transition: right 0.3s ease;
            display: flex;
            flex-direction: column;
            padding: 20px;
            box-shadow: -10px 0 30px rgba(0,0,0,0.5);
        }

        .side-drawer.open {
            right: 0;
        }

        .drawer-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
            margin-bottom: 15px;
        }

        .drawer-content {
            flex-grow: 1;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .history-item, .download-item {
            background: #1a1d26;
            padding: 10px;
            border-radius: 6px;
            font-size: 12px;
            border: 1px solid #252833;
        }

        .history-url {
            color: var(--accent-color);
            font-size: 11px;
            word-break: break-all;
        }
    </style>
</head>
<body>

    <div id="browser-ui">
        <!-- Aba de Controle das Guias -->
        <div id="tabs-bar">
            <!-- Abas renderizadas via JS -->
            <button class="add-tab-btn" onclick="browser.addTab()" title="Nova Aba">+</button>
        </div>

        <!-- Barra de Navegação -->
        <header>
            <div class="chromium-logo" onclick="browser.toggleOverview()" title="Neural-Raphael Core Overview">
                <div class="outer-ring"></div>
                <div class="center-circle"></div>
                <div class="segment seg1"></div>
                <div class="segment seg2"></div>
                <div class="segment seg3"></div>
            </div>

            <div class="nav-buttons">
                <button class="btn-icon" onclick="browser.goBack()" title="Voltar">◄</button>
                <button class="btn-icon" onclick="browser.goForward()" title="Avançar">►</button>
                <button class="btn-icon" onclick="browser.reload()" title="Recarregar">↻</button>
                <button class="btn-icon" onclick="browser.goHome()" title="Página Inicial">🏠</button>
            </div>

            <div id="address-container">
                <span class="protocol-icon" id="protocol-status">🔒</span>
                <input type="text" id="address-bar" value="neural://newtab" onkeydown="browser.handleUrlKey(event)">
                <select class="search-engine-selector" id="engine-select">
                    <option value="google">Google</option>
                    <option value="bing">Bing</option>
                    <option value="duck">DuckDuckGo</option>
                </select>
            </div>

            <div class="nav-buttons">
                <button class="btn-icon" onclick="browser.toggleOverview()" title="Visão Geral (Overview 2D)">🔳</button>
                <button class="btn-icon" onclick="browser.toggleDrawer('history')" title="Histórico">📜</button>
                <button class="btn-icon" onclick="browser.toggleDrawer('downloads')" title="Downloads">⬇️</button>
                <button class="btn-icon" onclick="browser.toggleIncognito()" id="incognito-btn" title="Modo Anônimo">🕶️</button>
            </div>
        </header>

        <!-- Motor Visual das Páginas -->
        <main id="viewport">
            <!-- Renderização dinâmica dos frames e NewTab -->
        </main>
    </div>

    <!-- VISÃO GERAL CANVAS OVERLAY -->
    <div id="overview-overlay">
        <div class="overview-header">
            <div class="overview-title">
                <div class="chromium-logo" style="transform: scale(0.8)">
                    <div class="outer-ring"></div>
                    <div class="center-circle"></div>
                    <div class="segment seg1"></div>
                    <div class="segment seg2"></div>
                    <div class="segment seg3"></div>
                </div>
                Visão Geral das Janelas do Sistema (Overview 2D Engine)
            </div>
            <button class="btn-icon" onclick="browser.toggleOverview()">✕</button>
        </div>
        <canvas id="overview-canvas"></canvas>
    </div>

    <!-- DRAWER LATERAL -->
    <div class="side-drawer" id="side-drawer">
        <div class="drawer-header">
            <h3 id="drawer-title" style="margin:0; font-size:16px;">Histórico</h3>
            <button class="btn-icon" onclick="browser.closeDrawer()">✕</button>
        </div>
        <div class="drawer-content" id="drawer-content">
            <!-- Conteúdo injetado via JS -->
        </div>
    </div>

    <script>
        /**
         * NEURAL-RAPHAEL-CROMIUM ENGINE CORE
         * Arquitetura Avançada de Navegador Baseado em Canvas, iFrames Proxied e Gerenciamento de Estado.
         */
        class NeuralCromiumEngine {
            constructor() {
                this.tabs = [];
                this.activeTabId = null;
                this.nextTabId = 1;
                this.historyList = [];
                this.downloadList = [];
                this.isIncognito = false;
                this.proxyPrefix = "https://api.allorigins.win/raw?url=";
                
                this.canvas = document.getElementById('overview-canvas');
                this.ctx = this.canvas.getContext('2d');
                
                this.initEngine();
            }

            initEngine() {
                this.bindEvents();
                this.addTab('Neural Home', 'neural://newtab');
                this.resizeCanvas();
                this.startCanvasRenderLoop();
            }

            bindEvents() {
                window.addEventListener('resize', () => this.resizeCanvas());
                
                this.canvas.addEventListener('click', (e) => this.handleCanvasClick(e));
                this.canvas.addEventListener('mousemove', (e) => this.handleCanvasHover(e));
            }

            // --- DEEP LINK & SEARCH ENGINE ---
            processUrl(input) {
                let url = input.trim();
                
                if (url.startsWith('neural://')) {
                    return url;
                }

                const isUrl = /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/.test(url);
                
                if (isUrl) {
                    if (!url.startsWith('http://') && !url.startsWith('https://')) {
                        url = 'https://' + url;
                    }
                    return url;
                }

                // Mecanismos de busca
                const engine = document.getElementById('engine-select').value;
                switch(engine) {
                    case 'bing':
                        return `https://www.bing.com/search?q=${encodeURIComponent(url)}`;
                    case 'duck':
                        return `https://duckduckgo.com/?q=${encodeURIComponent(url)}`;
                    default:
                        return `https://www.google.com/search?q=${encodeURIComponent(url)}`;
                }
            }

            // --- CONTROLE DE ABAS ---
            addTab(title = 'Nova Aba', url = 'neural://newtab') {
                const id = this.nextTabId++;
                const tab = {
                    id: id,
                    title: title,
                    url: url,
                    history: [url],
                    historyIndex: 0,
                    favicon: '🌐',
                    color: this.getRandomColor(),
                    thumbnailData: null,
                    isLoading: false
                };

                this.tabs.push(tab);
                this.createViewportFrame(tab);
                this.switchTab(id);
                this.renderTabsUI();
            }

            closeTab(id, event) {
                if(event) event.stopPropagation();
                if (this.tabs.length === 1) {
                    this.addTab();
                }

                const index = this.tabs.findIndex(t => t.id === id);
                const frame = document.getElementById(`frame-${id}`);
                if (frame) frame.remove();

                this.tabs = this.tabs.filter(t => t.id !== id);

                if (this.activeTabId === id) {
                    const nextActive = this.tabs[Math.max(0, index - 1)];
                    this.switchTab(nextActive.id);
                }

                this.renderTabsUI();
            }

            switchTab(id) {
                this.activeTabId = id;
                const tab = this.getTab(id);

                document.querySelectorAll('.frame-container').forEach(el => el.classList.remove('active'));
                const activeFrame = document.getElementById(`frame-${id}`);
                if (activeFrame) activeFrame.classList.add('active');

                document.getElementById('address-bar').value = tab.url;
                this.updateProtocolStatus(tab.url);
                this.renderTabsUI();
            }

            getTab(id) {
                return this.tabs.find(t => t.id === (id || this.activeTabId));
            }

            renderTabsUI() {
                const tabsBar = document.getElementById('tabs-bar');
                // Remove abas antigas sem deletar o botão '+'
                const oldTabs = tabsBar.querySelectorAll('.tab');
                oldTabs.forEach(t => t.remove());

                const addBtn = tabsBar.querySelector('.add-tab-btn');

                this.tabs.forEach(tab => {
                    const tabEl = document.createElement('div');
                    tabEl.className = `tab ${tab.id === this.activeTabId ? 'active' : ''}`;
                    tabEl.onclick = () => this.switchTab(tab.id);

                    tabEl.innerHTML = `
                        <span>${tab.favicon}</span>
                        <span class="tab-title">${tab.title}</span>
                        <span class="tab-close" onclick="browser.closeTab(${tab.id}, event)">✕</span>
                    `;

                    tabsBar.insertBefore(tabEl, addBtn);
                });
            }

            // --- VIEWPORT & IFRAME ENGINE ---
            createViewportFrame(tab) {
                const viewport = document.getElementById('viewport');
                const container = document.createElement('div');
                container.id = `frame-${tab.id}`;
                container.className = 'frame-container';

                if (tab.url === 'neural://newtab') {
                    container.innerHTML = this.getNewTabPageHTML();
                } else {
                    const iframe = document.createElement('iframe');
                    iframe.src = tab.url;
                    container.appendChild(iframe);
                }

                viewport.appendChild(container);
            }

            updateViewport(tab) {
                const container = document.getElementById(`frame-${tab.id}`);
                if (!container) return;

                if (!this.isIncognito) {
                    this.historyList.unshift({ title: tab.title, url: tab.url, time: new Date().toLocaleTimeString() });
                }

                if (tab.url === 'neural://newtab') {
                    container.innerHTML = this.getNewTabPageHTML();
                } else {
                    container.innerHTML = '';
                    const iframe = document.createElement('iframe');
                    iframe.src = tab.url;
                    
                    tab.isLoading = true;
                    iframe.onload = () => {
                        tab.isLoading = false;
                        try {
                            if (iframe.contentDocument && iframe.contentDocument.title) {
                                tab.title = iframe.contentDocument.title;
                                this.renderTabsUI();
                            }
                        } catch (e) {
                            // Cross-origin restriction workaround fallback
                            tab.title = this.extractDomain(tab.url);
                            this.renderTabsUI();
                        }
                    };

                    container.appendChild(iframe);
                }
            }

            getNewTabPageHTML() {
                return `
                    <div class="new-tab-page">
                        <div class="chromium-logo new-tab-logo">
                            <div class="outer-ring"></div>
                            <div class="center-circle"></div>
                            <div class="segment seg1"></div>
                            <div class="segment seg2"></div>
                            <div class="segment seg3"></div>
                        </div>
                        <h1 style="font-weight: 300; margin-bottom: 20px;">Neural-Raphael Core</h1>
                        <div class="search-box-large">
                            <input type="text" placeholder="Pesquisar na Web ou digitar URL..." onkeydown="if(event.key==='Enter'){ browser.navigateFromInput(this.value); }">
                        </div>
                        <div class="shortcuts-grid">
                            <div class="shortcut-card" onclick="browser.navigateFromInput('https://google.com')">
                                <div class="shortcut-icon">G</div>
                                <span>Google</span>
                            </div>
                            <div class="shortcut-card" onclick="browser.navigateFromInput('https://github.com')">
                                <div class="shortcut-icon">GH</div>
                                <span>GitHub</span>
                            </div>
                            <div class="shortcut-card" onclick="browser.navigateFromInput('https://wikipedia.org')">
                                <div class="shortcut-icon">W</div>
                                <span>Wikipedia</span>
                            </div>
                            <div class="shortcut-card" onclick="browser.navigateFromInput('https://youtube.com')">
                                <div class="shortcut-icon">Y</div>
                                <span>YouTube</span>
                            </div>
                        </div>
                    </div>
                `;
            }

            // --- NAVEGAÇÃO E CONTROLES ---
            handleUrlKey(e) {
                if (e.key === 'Enter') {
                    this.navigateFromInput(e.target.value);
                }
            }

            navigateFromInput(input) {
                const targetUrl = this.processUrl(input);
                const tab = this.getTab();
                
                tab.url = targetUrl;
                tab.title = this.extractDomain(targetUrl) || targetUrl;
                
                if (tab.historyIndex < tab.history.length - 1) {
                    tab.history = tab.history.slice(0, tab.historyIndex + 1);
                }
                tab.history.push(targetUrl);
                tab.historyIndex++;

                document.getElementById('address-bar').value = targetUrl;
                this.updateProtocolStatus(targetUrl);
                this.updateViewport(tab);
                this.renderTabsUI();
            }

            goBack() {
                const tab = this.getTab();
                if (tab.historyIndex > 0) {
                    tab.historyIndex--;
                    tab.url = tab.history[tab.historyIndex];
                    document.getElementById('address-bar').value = tab.url;
                    this.updateViewport(tab);
                    this.renderTabsUI();
                }
            }

            goForward() {
                const tab = this.getTab();
                if (tab.historyIndex < tab.history.length - 1) {
                    tab.historyIndex++;
                    tab.url = tab.history[tab.historyIndex];
                    document.getElementById('address-bar').value = tab.url;
                    this.updateViewport(tab);
                    this.renderTabsUI();
                }
            }

            reload() {
                const tab = this.getTab();
                this.updateViewport(tab);
            }

            goHome() {
                this.navigateFromInput('neural://newtab');
            }

            updateProtocolStatus(url) {
                const icon = document.getElementById('protocol-status');
                if (url.startsWith('https://')) {
                    icon.textContent = '🔒';
                    icon.style.color = 'var(--success-color)';
                } else if (url.startsWith('neural://')) {
                    icon.textContent = '⚡';
                    icon.style.color = 'var(--accent-color)';
                } else {
                    icon.textContent = '⚠️';
                    icon.style.color = '#ffb300';
                }
            }

            toggleIncognito() {
                this.isIncognito = !this.isIncognito;
                const btn = document.getElementById('incognito-btn');
                if (this.isIncognito) {
                    btn.style.background = 'var(--accent-color)';
                    alert('Modo Anônimo Ativado: O histórico não será salvo.');
                } else {
                    btn.style.background = '#20232c';
                    alert('Modo Anônimo Desativado.');
                }
            }

            // --- VISÃO GERAL (CANVAS 2D ENGINE) ---
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
                this.canvas.height = window.innerHeight - 80;
            }

            drawOverview() {
                this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

                const cardWidth = 280;
                const cardHeight = 180;
                const gap = 30;
                const startX = 60;
                let x = startX;
                let y = 40;

                this.tabs.forEach((tab) => {
                    const isActive = tab.id === this.activeTabId;

                    // Sombras e contornos de seleção
                    this.ctx.save();
                    if (isActive) {
                        this.ctx.shadowBlur = 20;
                        this.ctx.shadowColor = "rgba(255, 42, 42, 0.6)";
                        this.ctx.strokeStyle = "#ff2a2a";
                        this.ctx.lineWidth = 3;
                    } else {
                        this.ctx.shadowBlur = 10;
                        this.ctx.shadowColor = "rgba(0,0,0,0.5)";
                        this.ctx.strokeStyle = "#2d313c";
                        this.ctx.lineWidth = 1;
                    }

                    // Card Background
                    this.ctx.fillStyle = "#16181d";
                    this.ctx.beginPath();
                    this.ctx.roundRect(x, y, cardWidth, cardHeight, 12);
                    this.ctx.fill();
                    this.ctx.stroke();

                    // Header da janela embutida
                    this.ctx.fillStyle = tab.color;
                    this.ctx.beginPath();
                    this.ctx.roundRect(x, y, cardWidth, 32, [12, 12, 0, 0]);
                    this.ctx.fill();

                    // Título da aba
                    this.ctx.shadowBlur = 0;
                    this.ctx.fillStyle = "#ffffff";
                    this.ctx.font = "bold 12px 'Segoe UI', sans-serif";
                    const titleText = tab.title.length > 25 ? tab.title.substring(0, 25) + '...' : tab.title;
                    this.ctx.fillText(titleText, x + 12, y + 20);

                    // Conteúdo Simulado da Aba (Preview Virtual)
                    this.ctx.fillStyle = "#0d0e11";
                    this.ctx.fillRect(x + 10, y + 42, cardWidth - 20, cardHeight - 52);

                    // Desenhar representação esquemática do conteúdo
                    this.ctx.fillStyle = "#252833";
                    this.ctx.fillRect(x + 20, y + 55, 60, 8);
                    this.ctx.fillRect(x + 20, y + 70, cardWidth - 60, 6);
                    this.ctx.fillRect(x + 20, y + 82, cardWidth - 80, 6);
                    this.ctx.fillRect(x + 20, y + 94, cardWidth - 100, 6);

                    // Elemento gráfico
                    this.ctx.fillStyle = tab.color;
                    this.ctx.globalAlpha = 0.3;
                    this.ctx.beginPath();
                    this.ctx.arc(x + cardWidth - 40, y + 120, 20, 0, Math.PI * 2);
                    this.ctx.fill();
                    this.ctx.globalAlpha = 1.0;

                    // Guardar coordenadas de colisão para o evento de clique
                    tab.rect = { x, y, width: cardWidth, height: cardHeight };

                    this.ctx.restore();

                    // Calcular próxima posição na grade
                    x += cardWidth + gap;
                    if (x + cardWidth > this.canvas.width - startX) {
                        x = startX;
                        y += cardHeight + gap;
                    }
                });
            }

            startCanvasRenderLoop() {
                const loop = () => {
                    const overlay = document.getElementById('overview-overlay');
                    if (overlay.classList.contains('active')) {
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
                    if (tab.rect && 
                        mouseX >= tab.rect.x && mouseX <= tab.rect.x + tab.rect.width &&
                        mouseY >= tab.rect.y && mouseY <= tab.rect.y + tab.rect.height) {
                        
                        this.switchTab(tab.id);
                        this.toggleOverview();
                    }
                });
            }

            handleCanvasHover(e) {
                const rect = this.canvas.getBoundingClientRect();
                const mouseX = e.clientX - rect.left;
                const mouseY = e.clientY - rect.top;
                
                let isHovering = false;
                this.tabs.forEach(tab => {
                    if (tab.rect && 
                        mouseX >= tab.rect.x && mouseX <= tab.rect.x + tab.rect.width &&
                        mouseY >= tab.rect.y && mouseY <= tab.rect.y + tab.rect.height) {
                        isHovering = true;
                    }
                });

                this.canvas.style.cursor = isHovering ? 'pointer' : 'default';
            }

            // --- DRAWER LATERAL (HISTÓRICO & DOWNLOADS) ---
            toggleDrawer(type) {
                const drawer = document.getElementById('side-drawer');
                const title = document.getElementById('drawer-title');
                const content = document.getElementById('drawer-content');

                drawer.classList.add('open');
                content.innerHTML = '';

                if (type === 'history') {
                    title.textContent = 'Histórico de Navegação';
                    if (this.historyList.length === 0) {
                        content.innerHTML = '<p style="color:var(--text-dim); font-size:12px;">Nenhum histórico registrado.</p>';
                    } else {
                        this.historyList.forEach(item => {
                            const div = document.createElement('div');
                            div.className = 'history-item';
                            div.innerHTML = `
                                <strong>${item.title}</strong>
                                <div class="history-url">${item.url}</div>
                                <span style="font-size:9px; color:var(--text-dim);">${item.time}</span>
                            `;
                            div.onclick = () => this.navigateFromInput(item.url);
                            content.appendChild(div);
                        });
                    }
                } else if (type === 'downloads') {
                    title.textContent = 'Gerenciador de Downloads';
                    content.innerHTML = `
                        <div class="download-item">
                            <strong>Neural_Core_Update.bin</strong>
                            <div class="history-url">14.2 MB / 14.2 MB - Concluído</div>
                        </div>
                    `;
                }
            }

            closeDrawer() {
                document.getElementById('side-drawer').classList.remove('open');
            }

            // --- UTILITÁRIOS ---
            extractDomain(url) {
                try {
                    const parsed = new URL(url);
                    return parsed.hostname;
                } catch(e) {
                    return url;
                }
            }

            getRandomColor() {
                const colors = ['#ff2a2a', '#00d4ff', '#7000ff', '#ff007b', '#00e676', '#ffb300'];
                return colors[Math.floor(Math.random() * colors.length)];
            }
        }

        // Inicialização global do motor
        const browser = new NeuralCromiumEngine();
    </script>
</body>
</html>
