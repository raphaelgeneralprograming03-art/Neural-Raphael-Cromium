
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neural-Raphael-Cromium | Unified Architecture System</title>
    <style>
        :root {
            --bg-dark: #0a0b0d;
            --panel-bg: #14161b;
            --surface-bg: #1c1f26;
            --surface-border: #2d313d;
            --tab-bg: #1e2026;
            --toolbar-bg: #282a36;
            --accent-red: #ff2a2a;
            --accent-glow: rgba(255, 42, 42, 0.4);
            --accent-blue: #00d4ff;
            --text-main: #f1f1f5;
            --text-dim: #828896;
            --status-ok: #00e676;
            --status-warn: #ffb300;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; user-select: none; font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; }
        body, html { height: 100%; width: 100%; background: var(--bg-dark); color: var(--text-main); overflow: hidden; }

        #app-viewport { display: flex; flex-direction: column; height: 100vh; width: 100vw; }

        /* HEADER & WINDOW CONTROLS */
        #window-bar {
            height: 38px; background: #121318; display: flex; align-items: center; justify-content: space-between;
            padding: 0 12px; border-bottom: 1px solid var(--surface-border);
        }
        .window-title { font-size: 12px; font-weight: 600; color: var(--text-dim); display: flex; align-items: center; gap: 8px; }
        .logo-dot { width: 10px; height: 10px; background: var(--accent-red); border-radius: 50%; box-shadow: 0 0 8px var(--accent-red); }

        .system-metrics-badge {
            display: flex; align-items: center; gap: 12px; background: #0c0d11;
            padding: 4px 10px; border-radius: 6px; border: 1px solid var(--surface-border); font-size: 11px;
        }
        .metric-val { color: var(--accent-blue); font-weight: bold; }

        /* TAB BAR ENGINE */
        #tabs-bar { display: flex; align-items: flex-end; background: #16171d; padding: 6px 6px 0 6px; gap: 4px; height: 42px; overflow-x: auto; }
        .tab-item {
            display: flex; align-items: center; gap: 8px; background: var(--tab-bg); color: var(--text-dim);
            padding: 8px 14px; border-radius: 8px 8px 0 0; font-size: 12px; cursor: pointer; min-width: 160px; max-width: 220px;
            border: 1px solid transparent; border-bottom: none; transition: all 0.2s;
        }
        .tab-item.active { background: var(--toolbar-bg); color: var(--text-main); border-color: var(--surface-border); border-top: 2px solid var(--accent-red); }
        .tab-title { flex-grow: 1; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .tab-close { width: 16px; height: 16px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 10px; }
        .tab-close:hover { background: rgba(255,255,255,0.1); color: var(--accent-red); }

        .btn-add-tab { background: #20222a; color: #fff; border: 1px solid var(--surface-border); width: 28px; height: 28px; border-radius: 6px; cursor: pointer; margin-bottom: 4px; }

        /* NAVIGATION TOOLBAR */
        #toolbar { background: var(--toolbar-bg); padding: 8px 12px; display: flex; align-items: center; gap: 10px; border-bottom: 1px solid var(--surface-border); }
        .nav-btn {
            background: #1e2026; border: 1px solid var(--surface-border); color: #fff;
            padding: 6px 12px; border-radius: 6px; font-size: 12px; font-weight: 600; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 6px; transition: all 0.2s ease;
        }
        .nav-btn:hover { background: var(--accent-red); border-color: var(--accent-red); box-shadow: 0 0 10px var(--accent-glow); }

        #address-box { flex-grow: 1; display: flex; align-items: center; position: relative; }
        #address-input {
            width: 100%; background: #16171d; border: 1px solid var(--surface-border); color: #fff;
            padding: 8px 12px 8px 36px; border-radius: 20px; font-size: 13px; outline: none;
        }
        #address-input:focus { border-color: var(--accent-red); box-shadow: 0 0 10px rgba(255, 42, 42, 0.3); }
        .secure-icon { position: absolute; left: 12px; font-size: 12px; color: var(--status-ok); }

        /* BOOKMARKS BAR */
        #bookmarks-bar {
            background: #111216; padding: 4px 14px; display: flex; align-items: center; gap: 8px;
            border-bottom: 1px solid var(--surface-border); font-size: 11px; overflow-x: auto;
        }
        .bookmark-item {
            background: var(--panel-bg); border: 1px solid var(--surface-border); color: var(--text-dim);
            padding: 3px 8px; border-radius: 4px; cursor: pointer; display: flex; align-items: center; gap: 6px;
            white-space: nowrap; max-width: 180px; overflow: hidden; text-overflow: ellipsis;
        }
        .bookmark-item:hover { color: var(--text-main); border-color: var(--accent-red); }

        /* WORKSPACE & VIEWPORT */
        #workspace { flex-grow: 1; position: relative; width: 100%; height: 100%; overflow: hidden; background: #000; }
        .web-frame { width: 100%; height: 100%; border: none; display: none; background: #fff; }
        .web-frame.active { display: block; }

        /* OVERLAY PANELS SYSTEM */
        .system-panel {
            position: absolute; inset: 0; background: rgba(10, 11, 13, 0.96); backdrop-filter: blur(12px);
            z-index: 500; display: none; flex-direction: column; padding: 24px; overflow-y: auto;
        }
        .system-panel.active { display: flex; }

        .panel-header {
            display: flex; justify-content: space-between; align-items: center;
            border-bottom: 1px solid var(--surface-border); padding-bottom: 16px; margin-bottom: 20px;
        }
        .panel-title { font-size: 18px; font-weight: 700; color: var(--text-main); display: flex; align-items: center; gap: 10px; }

        /* DOWNLOADS MODULE */
        .download-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 16px; }
        .download-card { background: var(--panel-bg); border: 1px solid var(--surface-border); border-radius: 8px; padding: 16px; display: flex; flex-direction: column; gap: 10px; }
        .download-info { display: flex; justify-content: space-between; font-size: 12px; }
        .progress-track { height: 6px; background: var(--surface-bg); border-radius: 3px; overflow: hidden; }
        .progress-fill { height: 100%; background: var(--accent-red); width: 0%; transition: width 0.3s ease; }

        /* HISTORY & BOOKMARKS LIST */
        .history-controls { display: flex; gap: 12px; margin-bottom: 20px; }
        .search-input { flex-grow: 1; background: var(--panel-bg); border: 1px solid var(--surface-border); padding: 10px 16px; border-radius: 6px; color: #fff; outline: none; font-size: 13px; }
        .search-input:focus { border-color: var(--accent-red); }
        .history-list { display: flex; flex-direction: column; gap: 8px; }
        .history-row { background: var(--panel-bg); border: 1px solid var(--surface-border); padding: 12px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center; font-size: 13px; }
        .history-url { color: var(--accent-blue); font-size: 11px; text-decoration: none; }

        /* DEVTOOLS MODULE */
        .devtools-split { display: grid; grid-template-columns: 280px 1fr; gap: 20px; height: 100%; }
        .console-output { background: #050507; border: 1px solid var(--surface-border); border-radius: 8px; padding: 14px; font-family: monospace; font-size: 12px; color: var(--status-ok); overflow-y: auto; display: flex; flex-direction: column; gap: 6px; flex-grow: 1; }
        .console-line { border-bottom: 1px solid #111; padding-bottom: 4px; }
        .console-err { color: var(--accent-red); }
        .console-warn { color: var(--status-warn); }

        /* PROCESS MANAGER TABLE */
        table.process-table { width: 100%; border-collapse: collapse; text-align: left; font-size: 12px; }
        table.process-table th { background: var(--surface-bg); padding: 10px; color: var(--text-dim); border-bottom: 1px solid var(--surface-border); text-transform: uppercase; font-size: 10px; }
        table.process-table td { padding: 10px; border-bottom: 1px solid var(--surface-border); }
        table.process-table tr:hover { background: rgba(255, 255, 255, 0.03); }
        .pid-tag { font-family: monospace; color: var(--accent-blue); font-weight: bold; }
        .memory-bar { height: 4px; background: var(--surface-border); border-radius: 2px; width: 80px; overflow: hidden; display: inline-block; vertical-align: middle; margin-left: 8px; }
        .memory-fill { height: 100%; background: var(--status-ok); }

        /* SECURITY CARDS */
        .sec-card { background: var(--panel-bg); border: 1px solid var(--surface-border); border-radius: 8px; padding: 16px; margin-bottom: 12px; display: flex; align-items: center; justify-content: space-between; }
        .toggle-switch { position: relative; display: inline-block; width: 40px; height: 20px; }
        .toggle-switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #333; transition: .4s; border-radius: 20px; }
        .slider:before { position: absolute; content: ""; height: 14px; width: 14px; left: 3px; bottom: 3px; background-color: white; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--accent-red); }
        input:checked + .slider:before { transform: translateX(20px); }
    </style>
</head>
<body>

<div id="app-viewport">
    <!-- WINDOW BAR -->
    <div id="window-bar">
        <div class="window-title">
            <div class="logo-dot"></div>
            Neural-Raphael-Cromium Core 3.0 (Unified Engine)
        </div>
        <div class="system-metrics-badge">
            <div>CPU: <span class="metric-val" id="cpu-load">1.2%</span></div>
            <div>RAM: <span class="metric-val" id="ram-usage">342 MB</span></div>
            <div>Processos: <span class="metric-val" id="proc-count">4</span></div>
        </div>
    </div>

    <!-- TABS BAR -->
    <div id="tabs-bar">
        <button class="btn-add-tab" onclick="unifiedEngine.addTab()">+</button>
    </div>

    <!-- TOOLBAR -->
    <div id="toolbar">
        <button class="nav-btn" onclick="unifiedEngine.reload()">↻</button>
        <div id="address-box">
            <span class="secure-icon">🌐</span>
            <input type="text" id="address-input" placeholder="Digite uma URL..." onkeydown="unifiedEngine.handleKey(event)">
        </div>
        <button class="nav-btn" onclick="unifiedEngine.togglePanel('downloads-panel')">📥 Downloads (<span id="dl-count">0</span>)</button>
        <button class="nav-btn" onclick="unifiedEngine.togglePanel('history-panel')">📜 Histórico</button>
        <button class="nav-btn" onclick="unifiedEngine.togglePanel('bookmarks-panel')">⭐ Favoritos</button>
        <button class="nav-btn" onclick="unifiedEngine.togglePanel('devtools-panel')">🛠️ DevTools</button>
        <button class="nav-btn" onclick="unifiedEngine.togglePanel('taskmgr-panel')">⚙️ Processos</button>
        <button class="nav-btn" onclick="unifiedEngine.togglePanel('security-panel')">🛡️ Segurança</button>
    </div>

    <!-- BOOKMARKS BAR -->
    <div id="bookmarks-bar">
        <span style="color:var(--text-dim); font-weight:bold;">Acesso Rápido:</span>
        <div id="bookmarks-container" style="display:flex; gap:6px;"></div>
    </div>

    <!-- MAIN WORKSPACE -->
    <div id="workspace">
        
        <!-- PANEL: DOWNLOADS -->
        <div class="system-panel" id="downloads-panel">
            <div class="panel-header">
                <div class="panel-title">📥 Gerenciador de Downloads Direct Stream</div>
                <button class="nav-btn" onclick="unifiedEngine.togglePanel('downloads-panel')">✕ Fechar</button>
            </div>
            <div style="margin-bottom: 15px;">
                <button class="nav-btn" onclick="unifiedEngine.simulateDownload()">+ Iniciar Download de Teste</button>
            </div>
            <div class="download-grid" id="download-grid-target"></div>
        </div>

        <!-- PANEL: HISTORY -->
        <div class="system-panel" id="history-panel">
            <div class="panel-header">
                <div class="panel-title">📜 Histórico de Navegação (IndexedDB)</div>
                <button class="nav-btn" onclick="unifiedEngine.togglePanel('history-panel')">✕ Fechar</button>
            </div>
            <div class="history-controls">
                <input type="text" class="search-input" id="history-search" placeholder="Buscar no histórico..." oninput="unifiedEngine.filterHistory(this.value)">
                <button class="nav-btn" onclick="unifiedEngine.clearHistory()">Limpar Histórico</button>
            </div>
            <div class="history-list" id="history-list-target"></div>
        </div>

        <!-- PANEL: BOOKMARKS -->
        <div class="system-panel" id="bookmarks-panel">
            <div class="panel-header">
                <div class="panel-title">⭐ Gerenciador de Favoritos</div>
                <button class="nav-btn" onclick="unifiedEngine.togglePanel('bookmarks-panel')">✕ Fechar</button>
            </div>
            <div style="margin-bottom: 15px;">
                <button class="nav-btn" onclick="unifiedEngine.addBookmarkCurrent()">+ Favoritar URL Customizada</button>
            </div>
            <div class="history-list" id="bookmarks-panel-list"></div>
        </div>

        <!-- PANEL: DEVTOOLS -->
        <div class="system-panel" id="devtools-panel">
            <div class="panel-header">
                <div class="panel-title">🛠️ DevTools V8 & Sandbox de Extensões</div>
                <button class="nav-btn" onclick="unifiedEngine.togglePanel('devtools-panel')">✕ Fechar</button>
            </div>
            <div class="devtools-split">
                <div style="background: var(--panel-bg); border: 1px solid var(--surface-border); border-radius: 8px; padding: 12px;">
                    <h4 style="margin-bottom:12px;">Extensões Ativas</h4>
                    <div id="extension-items" style="display:flex; flex-direction:column; gap:8px;"></div>
                    <button class="nav-btn" style="width:100%; margin-top:15px;" onclick="unifiedEngine.loadExtensionPrompt()">+ Injetar Extensão</button>
                </div>
                <div style="display:flex; flex-direction:column; height:100%;">
                    <h4 style="margin-bottom:8px;">Console JS</h4>
                    <div class="console-output" id="devtools-console"></div>
                    <div style="display:flex; gap:8px; margin-top:8px;">
                        <input type="text" class="search-input" id="console-input" placeholder="Executar JS..." onkeydown="if(event.key==='Enter') unifiedEngine.evalConsole(this.value)">
                        <button class="nav-btn" onclick="unifiedEngine.evalConsole(document.getElementById('console-input').value)">Executar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- PANEL: TASK MANAGER -->
        <div class="system-panel" id="taskmgr-panel">
            <div class="panel-header">
                <div class="panel-title">⚙️ Gerenciador de Processos (Multi-Process Architecture)</div>
                <button class="nav-btn" onclick="unifiedEngine.togglePanel('taskmgr-panel')">✕ Fechar</button>
            </div>
            <table class="process-table">
                <thead>
                    <tr>
                        <th>PID</th>
                        <th>Tipo</th>
                        <th>Título</th>
                        <th>CPU</th>
                        <th>RAM</th>
                        <th>Sandbox</th>
                        <th>Ação</th>
                    </tr>
                </thead>
                <tbody id="process-table-body"></tbody>
            </table>
        </div>

        <!-- PANEL: SECURITY -->
        <div class="system-panel" id="security-panel">
            <div class="panel-header">
                <div class="panel-title">🛡️ Central de Segurança & Sandbox</div>
                <button class="nav-btn" onclick="unifiedEngine.togglePanel('security-panel')">✕ Fechar</button>
            </div>
            <div class="sec-card">
                <div>
                    <strong>Isolamento de Site (Site Isolation)</strong><br>
                    <span style="font-size:11px; color:var(--text-dim);">Executa cada site em um processo de memória isolado.</span>
                </div>
                <label class="toggle-switch"><input type="checkbox" checked><span class="slider"></span></label>
            </div>
            <div class="sec-card">
                <div>
                    <strong>Bloqueio de Pop-ups Inseguros</strong><br>
                    <span style="font-size:11px; color:var(--text-dim);">Impede criação não autorizada de janelas.</span>
                </div>
                <label class="toggle-switch"><input type="checkbox" checked><span class="slider"></span></label>
            </div>
            <div class="sec-card">
                <div>
                    <strong>Forçar Criptografia HTTPS</strong><br>
                    <span style="font-size:11px; color:var(--text-dim);">Exige certificados válidos para conexões de rede.</span>
                </div>
                <label class="toggle-switch"><input type="checkbox" checked><span class="slider"></span></label>
            </div>
        </div>

    </div>
</div>

<script>
    /**
     * NEURAL-RAPHAEL-CROMIUM - UNIFIED CORE ENGINE
     * Integração dos Módulos 1, 2 e 2.5/3
     */
    class UnifiedChromiumEngine {
        constructor() {
            this.tabs = [];
            this.activeTabId = null;
            this.nextTabId = 1;
            this.proxyEngine = "https://api.allorigins.win/raw?url=";
            this.db = null;

            this.downloads = [];
            this.bookmarks = [
                { id: 1, title: "Wikipedia Org", url: "https://wikipedia.org" },
                { id: 2, title: "GitHub Core", url: "https://github.com" }
            ];
            this.extensions = [
                { id: "ext-1", name: "AdBlocker Direct Shield", active: true },
                { id: "ext-2", name: "Network Inspector", active: true }
            ];

            this.processes = [
                { pid: 1001, type: "Browser Core", title: "Neural Host", cpu: "0.4%", ram: 145, isolated: true },
                { pid: 1002, type: "GPU Engine", title: "Hardware Accel", cpu: "0.2%", ram: 88, isolated: true },
                { pid: 1003, type: "Network Tunnel", title: "Direct Proxy", cpu: "0.1%", ram: 42, isolated: true }
            ];

            this.init();
        }

        init() {
            this.initDatabase();
            this.addTab("Wikipedia", "https://wikipedia.org");
            this.renderBookmarks();
            this.renderExtensions();
            this.startHardwareTelemetry();
            this.logConsole("Sistema Neural-Raphael-Cromium unificado carregado.", "info");
        }

        /* --- MODULE 1: TAB & FRAME MANAGEMENT --- */
        addTab(title = "Nova Guia", url = "https://wikipedia.org") {
            const id = this.nextTabId++;
            const tab = { id, title, url };
            this.tabs.push(tab);

            // Adiciona processo isolado para a aba
            const pid = 1000 + id;
            this.processes.push({
                pid: pid,
                type: "Tab Render",
                title: title,
                cpu: "0.2%",
                ram: 60,
                isolated: true
            });

            const viewport = document.getElementById("workspace");
            const iframe = document.createElement("iframe");
            iframe.className = "web-frame";
            iframe.id = `frame-${id}`;
            iframe.src = this.proxyEngine + encodeURIComponent(url);
            viewport.appendChild(iframe);

            this.switchTab(id);
            this.addHistoryRecord(url, title);
        }

        switchTab(id) {
            this.activeTabId = id;
            const tab = this.tabs.find(t => t.id === id);

            document.querySelectorAll(".web-frame").forEach(f => f.classList.remove("active"));
            const currentFrame = document.getElementById(`frame-${id}`);
            if (currentFrame) currentFrame.classList.add("active");

            if (tab) {
                document.getElementById("address-input").value = tab.url;
            }
            this.renderTabs();
        }

        closeTab(id, event) {
            if (event) event.stopPropagation();
            if (this.tabs.length === 1) return;

            this.tabs = this.tabs.filter(t => t.id !== id);
            this.processes = this.processes.filter(p => p.pid !== (1000 + id));

            const frame = document.getElementById(`frame-${id}`);
            if (frame) frame.remove();

            if (this.activeTabId === id) {
                this.switchTab(this.tabs[this.tabs.length - 1].id);
            } else {
                this.renderTabs();
            }
        }

        renderTabs() {
            const tabsBar = document.getElementById("tabs-bar");
            const btn = tabsBar.querySelector(".btn-add-tab");
            tabsBar.querySelectorAll(".tab-item").forEach(t => t.remove());

            this.tabs.forEach(tab => {
                const el = document.createElement("div");
                el.className = `tab-item ${tab.id === this.activeTabId ? "active" : ""}`;
                el.onclick = () => this.switchTab(tab.id);
                el.innerHTML = `
                    <span class="tab-title">${tab.title}</span>
                    <span class="tab-close" onclick="unifiedEngine.closeTab(${tab.id}, event)">✕</span>
                `;
                tabsBar.insertBefore(el, btn);
            });
        }

        handleKey(e) {
            if (e.key === "Enter") {
                let url = e.target.value;
                if (!url.startsWith("http")) url = "https://" + url;

                const tab = this.tabs.find(t => t.id === this.activeTabId);
                if (tab) {
                    tab.url = url;
                    tab.title = url.replace("https://", "").split("/")[0];

                    const frame = document.getElementById(`frame-${tab.id}`);
                    if (frame) frame.src = this.proxyEngine + encodeURIComponent(url);

                    // Atualiza processo correspondente
                    const proc = this.processes.find(p => p.pid === (1000 + tab.id));
                    if (proc) proc.title = tab.title;

                    this.renderTabs();
                    this.addHistoryRecord(url, tab.title);
                }
            }
        }

        reload() {
            const tab = this.tabs.find(t => t.id === this.activeTabId);
            if (tab) {
                const frame = document.getElementById(`frame-${tab.id}`);
                if (frame) frame.src = this.proxyEngine + encodeURIComponent(tab.url);
            }
        }

        /* --- MODULE 2: INDEXEDDB, HISTORY & BOOKMARKS --- */
        initDatabase() {
            const request = indexedDB.open("UnifiedCromiumDB", 1);
            request.onupgradeneeded = (e) => {
                this.db = e.target.result;
                if (!this.db.objectStoreNames.contains("history")) {
                    this.db.createObjectStore("history", { keyPath: "id", autoIncrement: true });
                }
            };
            request.onsuccess = (e) => {
                this.db = e.target.result;
                this.renderHistory();
            };
        }

        addHistoryRecord(url, title) {
            if (!this.db) return;
            const tx = this.db.transaction("history", "readwrite");
            tx.objectStore("history").add({
                url: url,
                title: title || url,
                timestamp: new Date().toLocaleString()
            });
            this.renderHistory();
        }

        renderHistory(filter = "") {
            if (!this.db) return;
            const tx = this.db.transaction("history", "readonly");
            const request = tx.objectStore("history").getAll();

            request.onsuccess = () => {
                const target = document.getElementById("history-list-target");
                target.innerHTML = "";
                const records = request.result.filter(r => 
                    r.url.toLowerCase().includes(filter.toLowerCase()) || 
                    r.title.toLowerCase().includes(filter.toLowerCase())
                );

                records.reverse().forEach(rec => {
                    const row = document.createElement("div");
                    row.className = "history-row";
                    row.innerHTML = `
                        <div>
                            <strong>${rec.title}</strong><br>
                            <span class="history-url">${rec.url}</span>
                        </div>
                        <span style="font-size:11px; color:var(--text-dim);">${rec.timestamp}</span>
                    `;
                    target.appendChild(row);
                });
            };
        }

        filterHistory(val) { this.renderHistory(val); }

        clearHistory() {
            if (!this.db) return;
            const tx = this.db.transaction("history", "readwrite");
            tx.objectStore("history").clear();
            this.renderHistory();
        }

        renderBookmarks() {
            const bar = document.getElementById("bookmarks-container");
            const panel = document.getElementById("bookmarks-panel-list");
            bar.innerHTML = "";
            panel.innerHTML = "";

            this.bookmarks.forEach(bm => {
                const item = document.createElement("div");
                item.className = "bookmark-item";
                item.innerHTML = `⭐ ${bm.title}`;
                item.onclick = () => this.addTab(bm.title, bm.url);
                bar.appendChild(item);

                const row = document.createElement("div");
                row.className = "history-row";
                row.innerHTML = `
                    <div><strong>${bm.title}</strong><br><span class="history-url">${bm.url}</span></div>
                    <button class="nav-btn" onclick="unifiedEngine.removeBookmark(${bm.id})">Remover</button>
                `;
                panel.appendChild(row);
            });
        }

        addBookmarkCurrent() {
            const url = prompt("URL para favoritar:", "https://google.com");
            if (url) {
                const title = url.replace("https://", "").split("/")[0];
                this.bookmarks.push({ id: Date.now(), title, url });
                this.renderBookmarks();
            }
        }

        removeBookmark(id) {
            this.bookmarks = this.bookmarks.filter(b => b.id !== id);
            this.renderBookmarks();
        }

        /* --- DOWNLOADS ENGINE --- */
        simulateDownload() {
            const id = Date.now();
            const name = `package_${Math.floor(Math.random()*1000)}.bin`;
            const dl = { id, name, progress: 0 };
            this.downloads.push(dl);
            document.getElementById("dl-count").textContent = this.downloads.length;

            const interval = setInterval(() => {
                dl.progress += 15;
                if (dl.progress >= 100) {
                    dl.progress = 100;
                    clearInterval(interval);
                }
                this.renderDownloads();
            }, 400);
        }

        renderDownloads() {
            const target = document.getElementById("download-grid-target");
            target.innerHTML = "";
            this.downloads.forEach(dl => {
                const card = document.createElement("div");
                card.className = "download-card";
                card.innerHTML = `
                    <div class="download-info"><strong>📦 ${dl.name}</strong><span>${dl.progress}%</span></div>
                    <div class="progress-track"><div class="progress-fill" style="width:${dl.progress}%"></div></div>
                `;
                target.appendChild(card);
            });
        }

        /* --- DEVTOOLS & CONSOLE --- */
        logConsole(msg, type = "info") {
            const box = document.getElementById("devtools-console");
            const line = document.createElement("div");
            line.className = `console-line ${type === 'err' ? 'console-err' : type === 'warn' ? 'console-warn' : ''}`;
            line.textContent = `> ${msg}`;
            box.appendChild(line);
            box.scrollTop = box.scrollHeight;
        }

        evalConsole(cmd) {
            if (!cmd) return;
            this.logConsole(`in: ${cmd}`, "info");
            try {
                const res = eval(cmd);
                this.logConsole(`out: ${res}`, "info");
            } catch (err) {
                this.logConsole(`error: ${err.message}`, "err");
            }
            document.getElementById("console-input").value = "";
        }

        renderExtensions() {
            const target = document.getElementById("extension-items");
            target.innerHTML = "";
            this.extensions.forEach(ext => {
                const item = document.createElement("div");
                item.className = "history-row";
                item.style.padding = "6px 10px";
                item.innerHTML = `
                    <span style="font-size:12px;">🧩 ${ext.name}</span>
                    <input type="checkbox" ${ext.active ? 'checked' : ''}>
                `;
                target.appendChild(item);
            });
        }

        loadExtensionPrompt() {
            const name = prompt("Nome da extensão:");
            if (name) {
                this.extensions.push({ id: `ext-${Date.now()}`, name, active: true });
                this.renderExtensions();
            }
        }

        /* --- MODULE 2.5/3: PROCESSES & HARDWARE METRICS --- */
        startHardwareTelemetry() {
            setInterval(() => {
                let totalRam = 0;
                this.processes.forEach(p => {
                    p.ram = Math.max(20, p.ram + (Math.floor(Math.random() * 3) - 1));
                    p.cpu = (Math.random() * 1.5).toFixed(1) + "%";
                    totalRam += p.ram;
                });

                document.getElementById("cpu-load").textContent = (Math.random() * 2 + 0.5).toFixed(1) + "%";
                document.getElementById("ram-usage").textContent = `${totalRam} MB`;
                document.getElementById("proc-count").textContent = this.processes.length;

                if (document.getElementById("taskmgr-panel").classList.contains("active")) {
                    this.renderProcessTable();
                }
            }, 2000);
        }

        renderProcessTable() {
            const tbody = document.getElementById("process-table-body");
            tbody.innerHTML = "";
            this.processes.forEach(p => {
                const tr = document.createElement("tr");
                tr.innerHTML = `
                    <td class="pid-tag">${p.pid}</td>
                    <td><strong>${p.type}</strong></td>
                    <td>${p.title}</td>
                    <td style="color:var(--accent-blue);">${p.cpu}</td>
                    <td>${p.ram} MB <div class="memory-bar"><div class="memory-fill" style="width:${Math.min(100, p.ram/2)}%"></div></div></td>
                    <td><span style="color:var(--status-ok)">🔒 Sandboxed</span></td>
                    <td>${p.pid > 1003 ? `<button class="nav-btn" style="padding:2px 6px; font-size:10px;" onclick="unifiedEngine.killProcess(${p.pid})">Kill</button>` : 'Protegido'}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        killProcess(pid) {
            const tabId = pid - 1000;
            this.closeTab(tabId);
            this.renderProcessTable();
        }

        /* --- UI PANEL TOGGLE --- */
        togglePanel(panelId) {
            const panel = document.getElementById(panelId);
            const isActive = panel.classList.contains("active");

            document.querySelectorAll(".system-panel").forEach(p => p.classList.remove("active"));
            if (!isActive) {
                panel.classList.add("active");
                if (panelId === "taskmgr-panel") this.renderProcessTable();
            }
        }
    }

    const unifiedEngine = new UnifiedChromiumEngine();
</script>
</body>
</html>
