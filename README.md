// Neural-Raphael-Cromium Core 3.0 - arquivo unico (app + interface)
// Executar: npx electron neural-raphael-cromium.js
const { app, BrowserWindow, session, shell } = require('electron');
const fs = require('fs');
const path = require('path');

function loadInterface() {
  const marker = '\n//@@' + 'HTML@@\n';
  const raw = fs.readFileSync(__filename, 'utf8').replace(/\r\n/g, '\n').split(marker)[1];
  const html = raw.split('\n').map(l => l.slice(2)).join('\n');
  const file = path.join(app.getPath('userData'), 'index.html');
  fs.mkdirSync(path.dirname(file), { recursive: true });
  fs.writeFileSync(file, html);
  return file;
}

function createWindow() {
  const win = new BrowserWindow({
    width: 1280, height: 800, backgroundColor: '#3a2a6b',
    title: 'Neural-Raphael-Cromium', autoHideMenuBar: true,
    webPreferences: { webSecurity: false, contextIsolation: true, nodeIntegration: false, sandbox: true }
  });
  // Permite que os sites carreguem dentro das abas (remove bloqueio de iframe)
  session.defaultSession.webRequest.onHeadersReceived((details, callback) => {
    const headers = {};
    for (const [key, value] of Object.entries(details.responseHeaders || {})) {
      const k = key.toLowerCase();
      if (k === 'x-frame-options') continue;
      if (k === 'content-security-policy' || k === 'content-security-policy-report-only') {
        headers[key] = value.map(v => v.replace(/frame-ancestors[^;]*;?/gi, ''));
        continue;
      }
      headers[key] = value;
    }
    callback({ responseHeaders: headers });
  });
  win.webContents.setWindowOpenHandler(({ url }) => { shell.openExternal(url); return { action: 'deny' }; });
  win.loadFile(loadInterface());
}

app.whenReady().then(createWindow);
app.on('window-all-closed', () => app.quit());

// ===== INTERFACE (HTML embutido; cada linha abaixo comeca com //) =====
//@@HTML@@
//<!DOCTYPE html>
//<html lang="pt-br">
//<head>
//<meta charset="UTF-8">
//<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
//<title>Neural-Raphael-Cromium | Unified Architecture System</title>
//<style>
//:root{
//--bg-dark:#3a2a6b;--panel-bg:#4b3a85;--surface-bg:#5b49a0;--surface-border:#8a76c9;
//--tab-bg:#6a56b0;--toolbar-bg:#9b84dd;--accent-red:#ff2a2a;--accent-glow:rgba(255,42,42,.4);
//--accent-blue:#00d4ff;--text-main:#f6f2ff;--text-dim:#d3c8f2;--status-ok:#00e676;--status-warn:#ffb300;
//--lilac-deep:#2e2058;--console-bg:#2a1d52;
//}
//*{box-sizing:border-box;margin:0;padding:0;font-family:'Segoe UI',system-ui,-apple-system,sans-serif}
//body,html{height:100%;width:100%;background:var(--bg-dark);color:var(--text-main);overflow:hidden}
//input,textarea{user-select:text}
//button{font-family:inherit}
//#app-viewport{display:flex;flex-direction:column;height:100%;width:100%}
//#window-bar{height:52px;background:var(--lilac-deep);display:flex;align-items:center;justify-content:space-between;padding:0 12px;border-bottom:1px solid var(--surface-border);gap:10px}
//.window-title{font-size:13px;font-weight:600;color:var(--text-dim);display:flex;align-items:center;gap:12px;min-width:0}
//.window-title span.t{white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
//.logo-dot{width:26px;height:26px;flex:none;background:var(--accent-red);border-radius:50%;box-shadow:0 0 14px var(--accent-red),0 0 0 4px rgba(255,42,42,.25)}
//.system-metrics-badge{display:flex;align-items:center;gap:12px;background:var(--console-bg);padding:4px 10px;border-radius:6px;border:1px solid var(--surface-border);font-size:11px;white-space:nowrap}
//.metric-val{color:var(--accent-blue);font-weight:bold}
//#tabs-bar{display:flex;align-items:flex-end;background:#4a3a7e;padding:6px 6px 0;gap:4px;height:42px;overflow-x:auto;flex:none}
//.tab-item{display:flex;align-items:center;gap:8px;background:var(--tab-bg);color:var(--text-dim);padding:8px 12px;border-radius:8px 8px 0 0;font-size:12px;cursor:pointer;min-width:130px;max-width:210px;border:1px solid transparent;border-bottom:none;transition:background .2s;flex:none}
//.tab-item.pinned{min-width:44px;max-width:44px;justify-content:center}
//.tab-item.pinned .tab-title,.tab-item.pinned .tab-close{display:none}
//.tab-item.active{background:var(--toolbar-bg);color:#fff;border-color:var(--surface-border);border-top:2px solid var(--accent-red)}
//.tab-title{flex-grow:1;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
//.tab-close{width:18px;height:18px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px}
//.tab-close:hover{background:rgba(255,255,255,.2);color:var(--accent-red)}
//.btn-add-tab{background:var(--tab-bg);color:#fff;border:1px solid var(--surface-border);width:28px;height:28px;border-radius:6px;cursor:pointer;margin-bottom:4px;flex:none}
//#toolbar{background:var(--toolbar-bg);padding:8px 12px;display:flex;align-items:center;gap:8px;border-bottom:1px solid var(--surface-border);overflow-x:auto;flex:none}
//.nav-btn{background:var(--panel-bg);border:1px solid var(--surface-border);color:#fff;padding:6px 11px;border-radius:6px;font-size:12px;font-weight:600;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:6px;transition:all .2s;white-space:nowrap;flex:none}
//.nav-btn:hover:not(:disabled){background:var(--accent-red);border-color:var(--accent-red);box-shadow:0 0 10px var(--accent-glow)}
//.nav-btn:disabled{opacity:.4;cursor:default}
//.nav-btn:focus-visible,.search-input:focus-visible{outline:2px solid #fff;outline-offset:1px}
//#address-box{flex:1 1 220px;min-width:180px;display:flex;align-items:center;position:relative}
//#address-input{width:100%;background:var(--lilac-deep);border:1px solid var(--surface-border);color:#fff;padding:8px 36px;border-radius:20px;font-size:13px;outline:none}
//#address-input:focus{border-color:var(--accent-red);box-shadow:0 0 10px rgba(255,42,42,.3)}
//.secure-icon{position:absolute;left:12px;font-size:12px}
//#star-btn{position:absolute;right:10px;background:none;border:none;color:#fff;font-size:15px;cursor:pointer}
//#load-bar{height:3px;background:var(--accent-red);width:0;transition:width .4s;flex:none}
//#bookmarks-bar{background:var(--lilac-deep);padding:4px 14px;display:flex;align-items:center;gap:8px;border-bottom:1px solid var(--surface-border);font-size:11px;overflow-x:auto;flex:none}
//.bookmark-item{background:var(--panel-bg);border:1px solid var(--surface-border);color:var(--text-dim);padding:3px 8px;border-radius:4px;cursor:pointer;white-space:nowrap;max-width:180px;overflow:hidden;text-overflow:ellipsis}
//.bookmark-item:hover{color:#fff;border-color:var(--accent-red)}
//#workspace{flex-grow:1;position:relative;width:100%;overflow:hidden;background:var(--bg-dark);min-height:0}
//.web-frame{position:absolute;top:0;left:0;width:100%;height:100%;border:none;display:none;background:#fff;transform-origin:0 0}
//.web-frame.active{display:block}
//#statusbar{height:22px;background:var(--lilac-deep);font-size:11px;color:var(--text-dim);display:flex;align-items:center;justify-content:space-between;padding:0 12px;flex:none;gap:10px}
//.system-panel{position:absolute;inset:0;background:rgba(58,42,107,.97);backdrop-filter:blur(12px);z-index:500;display:none;flex-direction:column;padding:20px;overflow-y:auto}
//.system-panel.active{display:flex}
//.panel-header{display:flex;justify-content:space-between;align-items:center;border-bottom:1px solid var(--surface-border);padding-bottom:14px;margin-bottom:16px;gap:10px}
//.panel-title{font-size:17px;font-weight:700;display:flex;align-items:center;gap:10px}
//.download-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:14px}
//.download-card{background:var(--panel-bg);border:1px solid var(--surface-border);border-radius:8px;padding:14px;display:flex;flex-direction:column;gap:10px}
//.download-info{display:flex;justify-content:space-between;font-size:12px}
//.progress-track{height:6px;background:var(--lilac-deep);border-radius:3px;overflow:hidden}
//.progress-fill{height:100%;background:var(--accent-red);transition:width .3s}
//.history-controls{display:flex;gap:10px;margin-bottom:16px;flex-wrap:wrap}
//.search-input{flex-grow:1;background:var(--lilac-deep);border:1px solid var(--surface-border);padding:10px 14px;border-radius:6px;color:#fff;outline:none;font-size:13px;min-width:0}
//.search-input:focus{border-color:var(--accent-red)}
//.history-list{display:flex;flex-direction:column;gap:8px}
//.history-row{background:var(--panel-bg);border:1px solid var(--surface-border);padding:10px 12px;border-radius:6px;display:flex;justify-content:space-between;align-items:center;font-size:13px;gap:10px}
//.history-row>div{min-width:0;overflow:hidden;text-overflow:ellipsis}
//.history-url{color:var(--accent-blue);font-size:11px;cursor:pointer}
//.empty{color:var(--text-dim);font-size:13px;padding:20px 0}
//.devtools-split{display:grid;grid-template-columns:240px 1fr;gap:16px;flex:1;min-height:0}
//@media(max-width:700px){.devtools-split{grid-template-columns:1fr}.system-metrics-badge{display:none}}
//.console-output{background:var(--console-bg);border:1px solid var(--surface-border);border-radius:8px;padding:12px;font-family:monospace;font-size:12px;color:var(--status-ok);overflow-y:auto;display:flex;flex-direction:column;gap:6px;flex-grow:1;min-height:140px}
//.console-line{border-bottom:1px solid var(--bg-dark);padding-bottom:4px;word-break:break-all}
//.console-err{color:#ff7b7b}.console-warn{color:var(--status-warn)}
//table.process-table{width:100%;border-collapse:collapse;text-align:left;font-size:12px}
//table.process-table th{background:var(--surface-bg);padding:10px;color:var(--text-dim);border-bottom:1px solid var(--surface-border);font-size:11px}
//table.process-table td{padding:10px;border-bottom:1px solid var(--surface-border)}
//.pid-tag{font-family:monospace;color:var(--accent-blue);font-weight:bold}
//.memory-bar{height:4px;background:var(--surface-border);border-radius:2px;width:70px;overflow:hidden;display:inline-block;vertical-align:middle;margin-left:8px}
//.memory-fill{height:100%;background:var(--status-ok)}
//.sec-card{background:var(--panel-bg);border:1px solid var(--surface-border);border-radius:8px;padding:14px;margin-bottom:12px;display:flex;align-items:center;justify-content:space-between;gap:12px}
//.sec-card small{font-size:11px;color:var(--text-dim)}
//.toggle-switch{position:relative;display:inline-block;width:40px;height:20px;flex:none}
//.toggle-switch input{opacity:0;width:0;height:0}
//.slider{position:absolute;cursor:pointer;inset:0;background:#2a1d52;transition:.3s;border-radius:20px}
//.slider:before{position:absolute;content:"";height:14px;width:14px;left:3px;bottom:3px;background:#fff;transition:.3s;border-radius:50%}
//input:checked+.slider{background:var(--accent-red)}
//input:checked+.slider:before{transform:translateX(20px)}
//select.search-input{flex-grow:0}
//textarea.search-input{width:100%;min-height:50vh;resize:vertical;font-family:inherit;line-height:1.5}
//#ctx{position:fixed;z-index:900;background:var(--panel-bg);border:1px solid var(--surface-border);border-radius:8px;padding:4px;display:none;min-width:170px;box-shadow:0 8px 24px rgba(0,0,0,.35)}
//#ctx button{display:block;width:100%;text-align:left;background:none;border:none;color:#fff;padding:8px 12px;font-size:12px;border-radius:5px;cursor:pointer}
//#ctx button:hover{background:var(--accent-red)}
//#toast{position:fixed;left:50%;bottom:34px;transform:translateX(-50%);background:var(--lilac-deep);border:1px solid var(--surface-border);padding:8px 16px;border-radius:20px;font-size:12px;opacity:0;pointer-events:none;transition:opacity .3s;z-index:950}
//#toast.show{opacity:1}
//</style>
//</head>
//<body>
//<div id="app-viewport">
//<div id="window-bar">
// <div class="window-title"><div class="logo-dot"></div><span class="t">Neural-Raphael-Cromium Core 3.0 (Unified Engine)</span></div>
// <div class="system-metrics-badge">
//  <div>CPU: <span class="metric-val" id="cpu-load">1.2%</span></div>
//  <div>RAM: <span class="metric-val" id="ram-usage">342 MB</span></div>
//  <div>Processos: <span class="metric-val" id="proc-count">4</span></div>
// </div>
//</div>
//<div id="tabs-bar"><button class="btn-add-tab" title="Nova guia (Ctrl+T)" onclick="unifiedEngine.addTab()">+</button></div>
//<div id="toolbar">
// <button class="nav-btn" id="back-btn" title="Voltar (Alt+←)" onclick="unifiedEngine.back()">←</button>
// <button class="nav-btn" id="fwd-btn" title="Avançar (Alt+→)" onclick="unifiedEngine.forward()">→</button>
// <button class="nav-btn" title="Recarregar (Ctrl+R)" onclick="unifiedEngine.reload()">↻</button>
// <button class="nav-btn" title="Início" onclick="unifiedEngine.navigate('about:newtab')">🏠</button>
// <button class="nav-btn" title="Abrir esta página numa janela externa" onclick="unifiedEngine.openExternal()">↗</button>
// <div id="address-box">
//  <span class="secure-icon" id="secure-icon">🌐</span>
//  <input type="text" id="address-input" placeholder="Pesquise ou digite uma URL (Ctrl+L)" autocomplete="off" onkeydown="unifiedEngine.handleKey(event)">
//  <button id="star-btn" title="Favoritar página" onclick="unifiedEngine.toggleBookmark()">☆</button>
// </div>
// <button class="nav-btn" title="Diminuir zoom" onclick="unifiedEngine.zoom(-0.1)">A−</button>
// <button class="nav-btn" title="Aumentar zoom" onclick="unifiedEngine.zoom(0.1)">A+</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('downloads-panel')">📥 Downloads (<span id="dl-count">0</span>)</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('history-panel')">📜 Histórico</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('bookmarks-panel')">⭐ Favoritos</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('notes-panel')">📝 Notas</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('devtools-panel')">🛠 DevTools</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('taskmgr-panel')">⚙️ Processos</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('security-panel')">🛡 Segurança</button>
// <button class="nav-btn" onclick="unifiedEngine.togglePanel('settings-panel')">🔧 Config.</button>
//</div>
//<div id="load-bar"></div>
//<div id="bookmarks-bar"><span style="color:var(--text-dim);font-weight:bold">Acesso Rápido:</span><div id="bookmarks-container" style="display:flex;gap:6px"></div></div>
//<div id="workspace">
// <div class="system-panel" id="downloads-panel">
//  <div class="panel-header"><div class="panel-title">📥 Gerenciador de Downloads Direct Stream</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('downloads-panel')">✕ Fechar</button></div>
//  <div style="margin-bottom:15px;display:flex;gap:8px;flex-wrap:wrap">
//   <button class="nav-btn" onclick="unifiedEngine.simulateDownload()">+ Iniciar Download de Teste</button>
//   <button class="nav-btn" onclick="unifiedEngine.clearDownloads()">Limpar concluídos</button>
//  </div>
//  <div class="download-grid" id="download-grid-target"></div>
// </div>
// <div class="system-panel" id="history-panel">
//  <div class="panel-header"><div class="panel-title">📜 Histórico de Navegação (IndexedDB)</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('history-panel')">✕ Fechar</button></div>
//  <div class="history-controls">
//   <input type="text" class="search-input" id="history-search" placeholder="Buscar no histórico..." oninput="unifiedEngine.filterHistory(this.value)">
//   <button class="nav-btn" onclick="unifiedEngine.clearHistory()">Limpar Histórico</button>
//  </div>
//  <div class="history-list" id="history-list-target"></div>
// </div>
// <div class="system-panel" id="bookmarks-panel">
//  <div class="panel-header"><div class="panel-title">⭐ Gerenciador de Favoritos</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('bookmarks-panel')">✕ Fechar</button></div>
//  <div style="margin-bottom:15px"><button class="nav-btn" onclick="unifiedEngine.addBookmarkCurrent()">+ Favoritar URL Customizada</button></div>
//  <div class="history-list" id="bookmarks-panel-list"></div>
// </div>
// <div class="system-panel" id="notes-panel">
//  <div class="panel-header"><div class="panel-title">📝 Bloco de Notas (salvo automaticamente)</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('notes-panel')">✕ Fechar</button></div>
//  <textarea class="search-input" id="notes-area" placeholder="Escreva suas anotações..." oninput="unifiedEngine.saveNotes(this.value)"></textarea>
// </div>
// <div class="system-panel" id="devtools-panel">
//  <div class="panel-header"><div class="panel-title">🛠 DevTools V8 & Sandbox de Extensões</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('devtools-panel')">✕ Fechar</button></div>
//  <div class="devtools-split">
//   <div style="background:var(--panel-bg);border:1px solid var(--surface-border);border-radius:8px;padding:12px;align-self:start">
//    <h4 style="margin-bottom:12px">Extensões Ativas</h4>
//    <div id="extension-items" style="display:flex;flex-direction:column;gap:8px"></div>
//    <button class="nav-btn" style="width:100%;margin-top:15px" onclick="unifiedEngine.loadExtensionPrompt()">+ Injetar Extensão</button>
//   </div>
//   <div style="display:flex;flex-direction:column;min-height:0">
//    <h4 style="margin-bottom:8px">Console JS</h4>
//    <div class="console-output" id="devtools-console"></div>
//    <div style="display:flex;gap:8px;margin-top:8px">
//     <input type="text" class="search-input" id="console-input" placeholder="Executar JS..." onkeydown="if(event.key==='Enter')unifiedEngine.evalConsole(this.value)">
//     <button class="nav-btn" onclick="unifiedEngine.evalConsole(document.getElementById('console-input').value)">Executar</button>
//     <button class="nav-btn" onclick="unifiedEngine.clearConsole()">Limpar</button>
//    </div>
//   </div>
//  </div>
// </div>
// <div class="system-panel" id="taskmgr-panel">
//  <div class="panel-header"><div class="panel-title">⚙️ Gerenciador de Processos (Multi-Process Architecture)</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('taskmgr-panel')">✕ Fechar</button></div>
//  <div style="overflow-x:auto"><table class="process-table"><thead><tr><th>PID</th><th>Tipo</th><th>Título</th><th>CPU</th><th>RAM</th><th>Sandbox</th><th>Ação</th></tr></thead><tbody id="process-table-body"></tbody></table></div>
// </div>
// <div class="system-panel" id="security-panel">
//  <div class="panel-header"><div class="panel-title">🛡 Central de Segurança & Sandbox</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('security-panel')">✕ Fechar</button></div>
//  <div id="security-cards"></div>
// </div>
// <div class="system-panel" id="settings-panel">
//  <div class="panel-header"><div class="panel-title">🔧 Configurações</div><button class="nav-btn" onclick="unifiedEngine.togglePanel('settings-panel')">✕ Fechar</button></div>
//  <div class="sec-card"><div><strong>Mecanismo de busca</strong><br><small>Usado quando você digita texto na barra de endereço.</small></div>
//   <select class="search-input" id="engine-select" onchange="unifiedEngine.setSetting('engine',this.value)">
//    <option value="duck">DuckDuckGo</option><option value="google">Google</option><option value="bing">Bing</option><option value="wiki">Wikipédia</option>
//   </select></div>
//  <div class="sec-card"><div><strong>Página inicial</strong><br><small>Aberta pelo botão 🏠 e em novas guias.</small></div>
//   <input class="search-input" id="home-input" style="max-width:260px" onchange="unifiedEngine.setSetting('home',this.value)"></div>
//  <div class="sec-card"><div><strong>Restaurar dados</strong><br><small>Apaga favoritos, notas e configurações salvas.</small></div>
//   <button class="nav-btn" onclick="unifiedEngine.resetAll()">Redefinir</button></div>
// </div>
//</div>
//<div id="statusbar"><span id="status-text">Pronto</span><span id="zoom-text">Zoom 100%</span></div>
//</div>
//<div id="ctx"></div>
//<div id="toast"></div>
//<script>
///** NEURAL-RAPHAEL-CROMIUM - UNIFIED CORE ENGINE (corrigido) */
//const esc = s => String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
//const store = {
//  get(k, d) { try { const v = localStorage.getItem('nrc:' + k); return v == null ? d : JSON.parse(v); } catch (e) { return d; } },
//  set(k, v) { try { localStorage.setItem('nrc:' + k, JSON.stringify(v)); } catch (e) {} },
//  clear() { try { Object.keys(localStorage).filter(k => k.startsWith('nrc:')).forEach(k => localStorage.removeItem(k)); } catch (e) {} }
//};
//const IS_ELECTRON = /Electron/i.test(navigator.userAgent);
//const ENGINES = {
//  duck: 'https://duckduckgo.com/?q=', google: 'https://www.google.com/search?q=',
//  bing: 'https://www.bing.com/search?q=', wiki: 'https://pt.wikipedia.org/w/index.php?search='
//};
//
//class UnifiedChromiumEngine {
//  constructor() {
//    this.tabs = []; this.activeTabId = null; this.nextTabId = 1; this.closedTabs = [];
//    this.proxyEngine = "https://api.allorigins.win/raw?url=";
//    this.viaProxy = u => IS_ELECTRON ? u : this.proxyEngine + encodeURIComponent(u);
//    this.db = null; this.memHistory = []; this.historyFilter = "";
//    this.downloads = [];
//    this.settings = Object.assign({ engine: 'duck', home: 'about:newtab' }, store.get('settings', {}));
//    this.bookmarks = store.get('bookmarks', [
//      { id: 1, title: "Wikipedia Org", url: "https://wikipedia.org" },
//      { id: 2, title: "GitHub Core", url: "https://github.com" }
//    ]);
//    this.extensions = store.get('extensions', [
//      { id: "ext-1", name: "AdBlocker Direct Shield", active: true },
//      { id: "ext-2", name: "Network Inspector", active: true }
//    ]);
//    this.security = store.get('security', { isolation: true, popups: true, https: true });
//    this.processes = [
//      { pid: 1001, type: "Browser Core", title: "Neural Host", cpu: "0.4%", ram: 145 },
//      { pid: 1002, type: "GPU Engine", title: "Hardware Accel", cpu: "0.2%", ram: 88 },
//      { pid: 1003, type: "Network Tunnel", title: "Direct Proxy", cpu: "0.1%", ram: 42 }
//    ];
//    this.init();
//  }
//  init() {
//    this.initDatabase();
//    this.renderBookmarks(); this.renderExtensions(); this.renderSecurity();
//    document.getElementById('engine-select').value = this.settings.engine;
//    document.getElementById('home-input').value = this.settings.home;
//    document.getElementById('notes-area').value = store.get('notes', '');
//    this.addTab("Nova Guia", this.settings.home);
//    this.startHardwareTelemetry(); this.bindShortcuts();
//    document.addEventListener('click', () => this.hideCtx());
//    this.logConsole("Sistema Neural-Raphael-Cromium unificado carregado.", "info");
//  }
//  toast(msg) {
//    const t = document.getElementById('toast'); t.textContent = msg; t.classList.add('show');
//    clearTimeout(this._tt); this._tt = setTimeout(() => t.classList.remove('show'), 2200);
//  }
//  setStatus(m) { document.getElementById('status-text').textContent = m; }
//
//  /* --- TABS & NAVIGATION --- */
//  getTab(id = this.activeTabId) { return this.tabs.find(t => t.id === id); }
//  tabPid(id) { return 2000 + id; }
//  addTab(title = "Nova Guia", url = this.settings.home) {
//    const id = this.nextTabId++;
//    const tab = { id, title, url: '', hist: [], idx: -1, zoom: 1, pinned: false };
//    this.tabs.push(tab);
//    this.processes.push({ pid: this.tabPid(id), type: "Tab Render", title, cpu: "0.2%", ram: 60 });
//    const iframe = document.createElement("iframe");
//    iframe.className = "web-frame"; iframe.id = `frame-${id}`;
//    iframe.setAttribute('referrerpolicy', 'no-referrer');
//    iframe.addEventListener('load', () => this.onFrameLoad(id));
//    document.getElementById("workspace").appendChild(iframe);
//    this.switchTab(id);
//    this.navigate(url, true, id);
//  }
//  resolveInput(raw) {
//    raw = raw.trim();
//    if (!raw || raw === 'about:newtab') return 'about:newtab';
//    if (/^https?:\/\//i.test(raw)) return raw;
//    if (!/\s/.test(raw) && /^[\w-]+(\.[\w-]+)+(:\d+)?(\/.*)?$/.test(raw)) return (this.security.https ? "https://" : "http://") + raw;
//    const e = this.settings.engine;
//    return (e === 'duck' || e === 'wiki') ? 'search:' + encodeURIComponent(raw) : ENGINES[e] + encodeURIComponent(raw);
//  }
//  titleFor(url) { return url === 'about:newtab' ? 'Nova Guia' : url.startsWith('search:') ? 'Busca: ' + decodeURIComponent(url.slice(7)) : url.replace(/^https?:\/\//, "").split("/")[0]; }
//  navigate(input, push = true, id = this.activeTabId) {
//    const tab = this.getTab(id); if (!tab) return;
//    const url = this.resolveInput(input);
//    if (push) { tab.hist = tab.hist.slice(0, tab.idx + 1); tab.hist.push(url); tab.idx = tab.hist.length - 1; }
//    tab.url = url; tab.title = this.titleFor(url);
//    this.loadFrame(tab);
//    const proc = this.processes.find(p => p.pid === this.tabPid(id)); if (proc) proc.title = tab.title;
//    if (url !== 'about:newtab') this.addHistoryRecord(url, tab.title);
//    this.renderTabs(); this.syncToolbar();
//  }
//  loadFrame(tab) {
//    const f = document.getElementById(`frame-${tab.id}`); if (!f) return;
//    this.progress(30); this.setStatus('Carregando ' + tab.url + '...');
//    if (tab.url.startsWith('search:')) { f.removeAttribute('src'); this.runSearch(tab, f); }
//    else if (tab.url === 'about:newtab') { f.removeAttribute('src'); f.srcdoc = this.newTabHTML(); }
//    else { f.removeAttribute('srcdoc'); f.src = this.viaProxy(tab.url); }
//  }
//  openExternal() {
//    const t = this.getTab(); if (!t || t.url === 'about:newtab') return;
//    const u = t.url.startsWith('search:') ? ENGINES.duck + t.url.slice(7) : t.url;
//    window.open(u, '_blank', 'noopener');
//  }
//  resultsHTML(q, items, note) {
//    const rows = items.map(r => `<div class="r"><a href="#" onclick="parent.unifiedEngine.navigate(${esc(JSON.stringify(r.url))});return false">${esc(r.title)}</a><cite>${esc(r.url)}</cite><p>${esc(r.snippet)}</p></div>`).join('');
//    return `<!DOCTYPE html><meta charset="utf-8"><style>body{margin:0;padding:24px;background:#f4f0ff;color:#2e2058;font-family:Segoe UI,system-ui,sans-serif}
//.w{max-width:720px;margin:auto}h2{font-size:15px;font-weight:600;color:#5b49a0;margin:0 0 18px}.r{background:#fff;border:1px solid #cfc2f2;border-radius:10px;padding:14px 16px;margin-bottom:12px}
//.r a{font-size:17px;color:#4b3a85;font-weight:600;text-decoration:none}.r a:hover{color:#ff2a2a}cite{display:block;font-size:12px;color:#00849e;margin:3px 0 6px;font-style:normal;word-break:break-all}.r p{margin:0;font-size:13px;line-height:1.5}</style>
//<div class="w"><h2>${esc(note)} "${esc(q)}"</h2>${rows || '<p>Nenhum resultado encontrado.</p>'}</div>`;
//  }
//  async searchDuck(q) {
//    const target = 'https://html.duckduckgo.com/html/?q=' + encodeURIComponent(q);
//    const res = await fetch(this.viaProxy(target));
//    if (!res.ok) throw new Error('HTTP ' + res.status);
//    const doc = new DOMParser().parseFromString(await res.text(), 'text/html');
//    const out = [];
//    doc.querySelectorAll('.result').forEach(el => {
//      const a = el.querySelector('.result__a'); if (!a) return;
//      let href = a.getAttribute('href') || '';
//      try { const u = new URL(href.startsWith('//') ? 'https:' + href : href, 'https://duckduckgo.com'); href = u.searchParams.get('uddg') || u.href; } catch (e) {}
//      if (!/^https?:/i.test(href)) return;
//      out.push({ title: a.textContent.trim(), url: href, snippet: (el.querySelector('.result__snippet') || {}).textContent || '' });
//    });
//    if (!out.length) throw new Error('sem resultados');
//    return out;
//  }
//  async searchWiki(q) {
//    const res = await fetch('https://pt.wikipedia.org/w/api.php?action=query&list=search&format=json&origin=*&srlimit=15&srsearch=' + encodeURIComponent(q));
//    if (!res.ok) throw new Error('HTTP ' + res.status);
//    const data = await res.json();
//    return data.query.search.map(r => ({ title: r.title, url: 'https://pt.wikipedia.org/wiki/' + encodeURIComponent(r.title.replace(/ /g, '_')), snippet: r.snippet.replace(/<[^>]+>/g, '').replace(/&quot;/g, '"').replace(/&amp;/g, '&') }));
//  }
//  async runSearch(tab, frame) {
//    const q = decodeURIComponent(tab.url.slice(7)), url = tab.url;
//    frame.srcdoc = this.resultsHTML(q, [], 'Buscando');
//    const order = this.settings.engine === 'wiki' ? ['wiki', 'duck'] : ['duck', 'wiki'];
//    let html = null, errs = [];
//    for (const eng of order) {
//      try {
//        const items = eng === 'duck' ? await this.searchDuck(q) : await this.searchWiki(q);
//        html = this.resultsHTML(q, items, (eng === 'duck' ? 'DuckDuckGo' : 'Wikipédia') + ': resultados para');
//        break;
//      } catch (e) { errs.push(`${eng}: ${e.message}`); this.logConsole(`Busca ${eng} falhou: ${e.message}`, 'warn'); }
//    }
//    if (tab.url !== url) return;
//    frame.srcdoc = html || this.resultsHTML(q, [], 'Não foi possível buscar. Verifique sua conexão e se o ambiente permite acesso externo. Erros: ' + errs.join(' | ') + '. Consulta:');
//  }
//  onFrameLoad(id) { if (id === this.activeTabId) { this.progress(100); this.setStatus('Pronto'); setTimeout(() => this.progress(0), 500); } }
//  progress(p) { const b = document.getElementById('load-bar'); b.style.opacity = p ? 1 : 0; b.style.width = p + '%'; }
//  newTabHTML() {
//    const quick = this.bookmarks.slice(0, 8).map(b => `<a href="#" onclick="parent.unifiedEngine.navigate(${esc(JSON.stringify(b.url))});return false">${esc(b.title)}</a>`).join('');
//    return `<!DOCTYPE html><meta charset="utf-8"><style>body{margin:0;height:100vh;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:22px;background:linear-gradient(160deg,#3a2a6b,#9b84dd);font-family:Segoe UI,system-ui,sans-serif;color:#f6f2ff}
//.dot{width:64px;height:64px;border-radius:50%;background:#ff2a2a;box-shadow:0 0 28px #ff2a2a}h1{margin:0;font-size:22px}
//form{width:min(520px,86vw)}input{width:100%;padding:13px 20px;border-radius:26px;border:1px solid #8a76c9;background:#2e2058;color:#fff;font-size:15px;outline:none}input:focus{border-color:#ff2a2a}
//.q{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;max-width:560px}.q a{background:#4b3a85;border:1px solid #8a76c9;color:#fff;padding:8px 14px;border-radius:8px;text-decoration:none;font-size:13px}.q a:hover{background:#ff2a2a}</style>
//<div class="dot"></div><h1>Neural-Raphael-Cromium</h1>
//<form onsubmit="parent.unifiedEngine.navigate(this.q.value);return false"><input name="q" placeholder="Pesquise ou digite uma URL" autofocus></form><div class="q">${quick}</div>`;
//  }
//  switchTab(id) {
//    this.activeTabId = id;
//    document.querySelectorAll(".web-frame").forEach(f => f.classList.remove("active"));
//    const fr = document.getElementById(`frame-${id}`); if (fr) fr.classList.add("active");
//    this.renderTabs(); this.syncToolbar();
//  }
//  closeTab(id, event) {
//    if (event) event.stopPropagation();
//    const tab = this.getTab(id); if (!tab) return;
//    if (this.tabs.length === 1) { this.toast('Não é possível fechar a última guia'); return; }
//    this.closedTabs.push({ title: tab.title, url: tab.url });
//    const i = this.tabs.indexOf(tab);
//    this.tabs.splice(i, 1);
//    this.processes = this.processes.filter(p => p.pid !== this.tabPid(id));
//    const f = document.getElementById(`frame-${id}`); if (f) f.remove();
//    if (this.activeTabId === id) this.switchTab(this.tabs[Math.min(i, this.tabs.length - 1)].id);
//    else this.renderTabs();
//  }
//  reopenTab() {
//    const c = this.closedTabs.pop();
//    if (c) this.addTab(c.title, c.url); else this.toast('Nenhuma guia fechada recentemente');
//  }
//  duplicateTab(id) { const t = this.getTab(id); if (t) this.addTab(t.title, t.url); }
//  pinTab(id) { const t = this.getTab(id); if (!t) return; t.pinned = !t.pinned; this.tabs.sort((a, b) => b.pinned - a.pinned); this.renderTabs(); }
//  closeOthers(id) { this.tabs.map(t => t.id).filter(x => x !== id && !this.getTab(x).pinned).forEach(x => this.closeTab(x)); this.switchTab(id); }
//  renderTabs() {
//    const bar = document.getElementById("tabs-bar"), btn = bar.querySelector(".btn-add-tab");
//    bar.querySelectorAll(".tab-item").forEach(t => t.remove());
//    this.tabs.forEach(tab => {
//      const el = document.createElement("div");
//      el.className = `tab-item ${tab.id === this.activeTabId ? "active" : ""} ${tab.pinned ? "pinned" : ""}`;
//      el.title = tab.url;
//      el.onclick = () => this.switchTab(tab.id);
//      el.onauxclick = e => { if (e.button === 1) this.closeTab(tab.id, e); };
//      el.oncontextmenu = e => { e.preventDefault(); this.showCtx(e, [
//        ['Duplicar guia', () => this.duplicateTab(tab.id)],
//        [tab.pinned ? 'Desafixar guia' : 'Fixar guia', () => this.pinTab(tab.id)],
//        ['Fechar outras guias', () => this.closeOthers(tab.id)],
//        ['Reabrir guia fechada', () => this.reopenTab()],
//        ['Fechar guia', () => this.closeTab(tab.id)]]); };
//      el.innerHTML = `<span>${tab.pinned ? '📌' : '🌐'}</span><span class="tab-title">${esc(tab.title)}</span><span class="tab-close">✕</span>`;
//      el.querySelector('.tab-close').onclick = e => this.closeTab(tab.id, e);
//      bar.insertBefore(el, btn);
//    });
//  }
//  showCtx(e, items) {
//    const m = document.getElementById('ctx'); m.innerHTML = '';
//    items.forEach(([label, fn]) => { const b = document.createElement('button'); b.textContent = label; b.onclick = fn; m.appendChild(b); });
//    m.style.display = 'block';
//    m.style.left = Math.min(e.clientX, innerWidth - 190) + 'px';
//    m.style.top = Math.min(e.clientY, innerHeight - items.length * 34 - 10) + 'px';
//  }
//  hideCtx() { document.getElementById('ctx').style.display = 'none'; }
//  syncToolbar() {
//    const tab = this.getTab(); if (!tab) return;
//    document.getElementById("address-input").value = tab.url === 'about:newtab' ? '' : tab.url.startsWith('search:') ? decodeURIComponent(tab.url.slice(7)) : tab.url;
//    document.getElementById('back-btn').disabled = tab.idx <= 0;
//    document.getElementById('fwd-btn').disabled = tab.idx >= tab.hist.length - 1;
//    document.getElementById('secure-icon').textContent = tab.url.startsWith('https://') ? '🔒' : '🌐';
//    document.getElementById('star-btn').textContent = this.bookmarks.some(b => b.url === tab.url) ? '★' : '☆';
//    this.applyZoom(tab);
//  }
//  handleKey(e) { if (e.key === "Enter") { this.navigate(e.target.value); e.target.blur(); } }
//  back() { const t = this.getTab(); if (t && t.idx > 0) { t.idx--; this.navigate(t.hist[t.idx], false); } }
//  forward() { const t = this.getTab(); if (t && t.idx < t.hist.length - 1) { t.idx++; this.navigate(t.hist[t.idx], false); } }
//  reload() { const t = this.getTab(); if (t) this.loadFrame(t); }
//  zoom(d) { const t = this.getTab(); if (!t) return; t.zoom = Math.min(2, Math.max(0.5, Math.round((t.zoom + d) * 10) / 10)); this.applyZoom(t); }
//  applyZoom(t) {
//    const f = document.getElementById(`frame-${t.id}`); if (!f) return;
//    f.style.transform = `scale(${t.zoom})`; f.style.width = f.style.height = (100 / t.zoom) + '%';
//    document.getElementById('zoom-text').textContent = `Zoom ${Math.round(t.zoom * 100)}%`;
//  }
//  bindShortcuts() {
//    document.addEventListener('keydown', e => {
//      const k = e.key.toLowerCase(), c = e.ctrlKey || e.metaKey;
//      if (c && e.shiftKey && k === 't') { e.preventDefault(); this.reopenTab(); }
//      else if (c && k === 't') { e.preventDefault(); this.addTab(); }
//      else if (c && k === 'w') { e.preventDefault(); this.closeTab(this.activeTabId); }
//      else if (c && k === 'l') { e.preventDefault(); const a = document.getElementById('address-input'); a.focus(); a.select(); }
//      else if (c && k === 'r') { e.preventDefault(); this.reload(); }
//      else if (c && k === 'd') { e.preventDefault(); this.toggleBookmark(); }
//      else if (c && (k === '+' || k === '=')) { e.preventDefault(); this.zoom(0.1); }
//      else if (c && k === '-') { e.preventDefault(); this.zoom(-0.1); }
//      else if (e.altKey && e.key === 'ArrowLeft') this.back();
//      else if (e.altKey && e.key === 'ArrowRight') this.forward();
//      else if (e.key === 'Escape') document.querySelectorAll('.system-panel').forEach(p => p.classList.remove('active'));
//    });
//  }
//
//  /* --- HISTORY (IndexedDB com fallback em memória) --- */
//  initDatabase() {
//    try {
//      const req = indexedDB.open("UnifiedCromiumDB", 1);
//      req.onupgradeneeded = e => { const db = e.target.result; if (!db.objectStoreNames.contains("history")) db.createObjectStore("history", { keyPath: "id", autoIncrement: true }); };
//      req.onsuccess = e => { this.db = e.target.result; this.renderHistory(); };
//      req.onerror = () => this.logConsole("IndexedDB indisponível; histórico em memória.", "warn");
//    } catch (err) { this.logConsole("IndexedDB indisponível; histórico em memória.", "warn"); }
//  }
//  addHistoryRecord(url, title) {
//    const rec = { url, title: title || url, timestamp: new Date().toLocaleString() };
//    if (!this.db) { rec.id = Date.now() + Math.random(); this.memHistory.push(rec); this.renderHistory(); return; }
//    const tx = this.db.transaction("history", "readwrite");
//    tx.objectStore("history").add(rec);
//    tx.oncomplete = () => this.renderHistory();
//  }
//  getHistory(cb) {
//    if (!this.db) return cb(this.memHistory.slice());
//    const r = this.db.transaction("history", "readonly").objectStore("history").getAll();
//    r.onsuccess = () => cb(r.result);
//  }
//  renderHistory() {
//    const f = this.historyFilter.toLowerCase();
//    this.getHistory(all => {
//      const target = document.getElementById("history-list-target"); target.innerHTML = "";
//      const recs = all.filter(r => r.url.toLowerCase().includes(f) || r.title.toLowerCase().includes(f)).reverse();
//      if (!recs.length) { target.innerHTML = '<div class="empty">Nenhum registro encontrado.</div>'; return; }
//      recs.forEach(rec => {
//        const row = document.createElement("div"); row.className = "history-row";
//        row.innerHTML = `<div><strong>${esc(rec.title)}</strong><br><span class="history-url">${esc(rec.url)}</span></div>
//          <span style="font-size:11px;color:var(--text-dim);white-space:nowrap">${esc(rec.timestamp)}</span><button class="nav-btn" style="padding:2px 8px">✕</button>`;
//        row.querySelector('.history-url').onclick = () => { this.togglePanel('history-panel'); this.navigate(rec.url); };
//        row.querySelector('button').onclick = () => this.deleteHistory(rec.id);
//        target.appendChild(row);
//      });
//    });
//  }
//  deleteHistory(id) {
//    if (!this.db) { this.memHistory = this.memHistory.filter(r => r.id !== id); return this.renderHistory(); }
//    const tx = this.db.transaction("history", "readwrite"); tx.objectStore("history").delete(id); tx.oncomplete = () => this.renderHistory();
//  }
//  filterHistory(v) { this.historyFilter = v; this.renderHistory(); }
//  clearHistory() {
//    if (!this.db) { this.memHistory = []; return this.renderHistory(); }
//    const tx = this.db.transaction("history", "readwrite"); tx.objectStore("history").clear(); tx.oncomplete = () => this.renderHistory();
//  }
//
//  /* --- BOOKMARKS --- */
//  saveBookmarks() { store.set('bookmarks', this.bookmarks); }
//  renderBookmarks() {
//    const bar = document.getElementById("bookmarks-container"), panel = document.getElementById("bookmarks-panel-list");
//    bar.innerHTML = ""; panel.innerHTML = "";
//    if (!this.bookmarks.length) panel.innerHTML = '<div class="empty">Nenhum favorito. Use ☆ na barra de endereço.</div>';
//    this.bookmarks.forEach(bm => {
//      const item = document.createElement("div"); item.className = "bookmark-item"; item.textContent = `⭐ ${bm.title}`;
//      item.onclick = () => this.addTab(bm.title, bm.url); bar.appendChild(item);
//      const row = document.createElement("div"); row.className = "history-row";
//      row.innerHTML = `<div><strong>${esc(bm.title)}</strong><br><span class="history-url">${esc(bm.url)}</span></div><button class="nav-btn">Remover</button>`;
//      row.querySelector('button').onclick = () => this.removeBookmark(bm.id);
//      row.querySelector('.history-url').onclick = () => { this.togglePanel('bookmarks-panel'); this.navigate(bm.url); };
//      panel.appendChild(row);
//    });
//    this.syncToolbar();
//  }
//  toggleBookmark() {
//    const t = this.getTab(); if (!t || t.url === 'about:newtab') return this.toast('Abra uma página para favoritar');
//    const ex = this.bookmarks.find(b => b.url === t.url);
//    if (ex) { this.removeBookmark(ex.id); this.toast('Favorito removido'); }
//    else { this.bookmarks.push({ id: Date.now(), title: t.title, url: t.url }); this.saveBookmarks(); this.renderBookmarks(); this.toast('Página favoritada ⭐'); }
//  }
//  addBookmarkCurrent() {
//    const raw = prompt("URL para favoritar:", "https://google.com");
//    if (raw) { const url = this.resolveInput(raw); this.bookmarks.push({ id: Date.now(), title: this.titleFor(url), url }); this.saveBookmarks(); this.renderBookmarks(); }
//  }
//  removeBookmark(id) { this.bookmarks = this.bookmarks.filter(b => b.id !== id); this.saveBookmarks(); this.renderBookmarks(); }
//
//  /* --- DOWNLOADS (pausar / cancelar / remover) --- */
//  simulateDownload() {
//    const dl = { id: Date.now() + Math.random(), name: `package_${Math.floor(Math.random() * 1000)}.bin`, progress: 0, state: 'running' };
//    this.downloads.push(dl);
//    dl.timer = setInterval(() => {
//      if (dl.state !== 'running') return;
//      dl.progress = Math.min(100, dl.progress + 5 + Math.floor(Math.random() * 10));
//      if (dl.progress >= 100) { dl.state = 'done'; clearInterval(dl.timer); this.toast(`Download concluído: ${dl.name}`); }
//      this.renderDownloads();
//    }, 400);
//    this.renderDownloads();
//  }
//  dlAction(id, act) {
//    const d = this.downloads.find(x => x.id === id); if (!d) return;
//    if (act === 'pause') d.state = d.state === 'paused' ? 'running' : 'paused';
//    else if (act === 'cancel') { clearInterval(d.timer); d.state = 'canceled'; }
//    else if (act === 'remove') { clearInterval(d.timer); this.downloads = this.downloads.filter(x => x !== d); }
//    this.renderDownloads();
//  }
//  clearDownloads() { this.downloads.filter(d => d.state !== 'running' && d.state !== 'paused').forEach(d => this.dlAction(d.id, 'remove')); }
//  renderDownloads() {
//    const target = document.getElementById("download-grid-target"); target.innerHTML = "";
//    document.getElementById("dl-count").textContent = this.downloads.length;
//    if (!this.downloads.length) target.innerHTML = '<div class="empty">Nenhum download. Inicie um download de teste.</div>';
//    const label = { running: 'Baixando', paused: 'Pausado', done: 'Concluído', canceled: 'Cancelado' };
//    this.downloads.forEach(dl => {
//      const card = document.createElement("div"); card.className = "download-card";
//      const active = dl.state === 'running' || dl.state === 'paused';
//      card.innerHTML = `<div class="download-info"><strong>📦 ${esc(dl.name)}</strong><span>${dl.progress}%</span></div>
//        <div class="progress-track"><div class="progress-fill" style="width:${dl.progress}%"></div></div>
//        <div class="download-info"><span>${label[dl.state]}</span><span style="display:flex;gap:6px">
//        ${active ? `<button class="nav-btn" data-a="pause" style="padding:2px 8px">${dl.state === 'paused' ? 'Retomar' : 'Pausar'}</button><button class="nav-btn" data-a="cancel" style="padding:2px 8px">Cancelar</button>` : `<button class="nav-btn" data-a="remove" style="padding:2px 8px">Remover</button>`}</span></div>`;
//      card.querySelectorAll('button').forEach(b => b.onclick = () => this.dlAction(dl.id, b.dataset.a));
//      target.appendChild(card);
//    });
//  }
//
//  /* --- DEVTOOLS --- */
//  logConsole(msg, type = "info") {
//    const box = document.getElementById("devtools-console"), line = document.createElement("div");
//    line.className = `console-line ${type === 'err' ? 'console-err' : type === 'warn' ? 'console-warn' : ''}`;
//    line.textContent = `> ${msg}`; box.appendChild(line); box.scrollTop = box.scrollHeight;
//  }
//  clearConsole() { document.getElementById("devtools-console").innerHTML = ""; }
//  evalConsole(cmd) {
//    if (!cmd) return;
//    this.logConsole(`in: ${cmd}`);
//    try { const res = (0, eval)(cmd); this.logConsole(`out: ${typeof res === 'object' ? JSON.stringify(res) : res}`); }
//    catch (err) { this.logConsole(`error: ${err.message}`, "err"); }
//    document.getElementById("console-input").value = "";
//  }
//  renderExtensions() {
//    const target = document.getElementById("extension-items"); target.innerHTML = "";
//    this.extensions.forEach(ext => {
//      const item = document.createElement("div"); item.className = "history-row"; item.style.padding = "6px 10px";
//      item.innerHTML = `<span style="font-size:12px">🧩 ${esc(ext.name)}</span><span style="display:flex;gap:6px;align-items:center"><input type="checkbox" ${ext.active ? 'checked' : ''}><button class="nav-btn" style="padding:0 6px">✕</button></span>`;
//      item.querySelector('input').onchange = e => { ext.active = e.target.checked; store.set('extensions', this.extensions); this.logConsole(`Extensão "${ext.name}" ${ext.active ? 'ativada' : 'desativada'}.`, ext.active ? 'info' : 'warn'); };
//      item.querySelector('button').onclick = () => { this.extensions = this.extensions.filter(x => x !== ext); store.set('extensions', this.extensions); this.renderExtensions(); };
//      target.appendChild(item);
//    });
//  }
//  loadExtensionPrompt() {
//    const name = prompt("Nome da extensão:");
//    if (name) { this.extensions.push({ id: `ext-${Date.now()}`, name, active: true }); store.set('extensions', this.extensions); this.renderExtensions(); }
//  }
//
//  /* --- PROCESSES & METRICS --- */
//  startHardwareTelemetry() {
//    setInterval(() => {
//      let total = 0;
//      this.processes.forEach(p => { p.ram = Math.max(20, p.ram + Math.floor(Math.random() * 3) - 1); p.cpu = (Math.random() * 1.5).toFixed(1) + "%"; total += p.ram; });
//      document.getElementById("cpu-load").textContent = (Math.random() * 2 + 0.5).toFixed(1) + "%";
//      document.getElementById("ram-usage").textContent = `${total} MB`;
//      document.getElementById("proc-count").textContent = this.processes.length;
//      if (document.getElementById("taskmgr-panel").classList.contains("active")) this.renderProcessTable();
//    }, 2000);
//  }
//  renderProcessTable() {
//    const tbody = document.getElementById("process-table-body"); tbody.innerHTML = "";
//    this.processes.forEach(p => {
//      const tr = document.createElement("tr"), tab = p.pid >= 2000;
//      tr.innerHTML = `<td class="pid-tag">${p.pid}</td><td><strong>${esc(p.type)}</strong></td><td>${esc(p.title)}</td><td style="color:var(--accent-blue)">${p.cpu}</td>
//        <td>${p.ram} MB <div class="memory-bar"><div class="memory-fill" style="width:${Math.min(100, p.ram / 2)}%"></div></div></td>
//        <td><span style="color:var(--status-ok)">🔒 Sandboxed</span></td><td>${tab ? '<button class="nav-btn" style="padding:2px 6px;font-size:10px">Kill</button>' : 'Protegido'}</td>`;
//      if (tab) tr.querySelector('button').onclick = () => this.killProcess(p.pid);
//      tbody.appendChild(tr);
//    });
//  }
//  killProcess(pid) { this.closeTab(pid - 2000); this.renderProcessTable(); }
//
//  /* --- SECURITY & SETTINGS --- */
//  renderSecurity() {
//    const defs = [['isolation', 'Isolamento de Site (Site Isolation)', 'Executa cada site em um processo de memória isolado.'],
//      ['popups', 'Bloqueio de Pop-ups Inseguros', 'Impede criação não autorizada de janelas.'],
//      ['https', 'Forçar Criptografia HTTPS', 'Endereços sem protocolo abrem com https://.']];
//    const box = document.getElementById('security-cards'); box.innerHTML = '';
//    defs.forEach(([k, t, d]) => {
//      const c = document.createElement('div'); c.className = 'sec-card';
//      c.innerHTML = `<div><strong>${t}</strong><br><small>${d}</small></div><label class="toggle-switch"><input type="checkbox" ${this.security[k] ? 'checked' : ''}><span class="slider"></span></label>`;
//      c.querySelector('input').onchange = e => { this.security[k] = e.target.checked; store.set('security', this.security); this.logConsole(`Segurança: ${t} = ${e.target.checked}`, e.target.checked ? 'info' : 'warn'); };
//      box.appendChild(c);
//    });
//  }
//  setSetting(k, v) { this.settings[k] = v; store.set('settings', this.settings); this.toast('Configuração salva'); }
//  saveNotes(v) { store.set('notes', v); }
//  resetAll() { if (confirm('Apagar favoritos, notas e configurações?')) { store.clear(); location.reload(); } }
//
//  togglePanel(id) {
//    const panel = document.getElementById(id), was = panel.classList.contains("active");
//    document.querySelectorAll(".system-panel").forEach(p => p.classList.remove("active"));
//    if (!was) { panel.classList.add("active"); if (id === "taskmgr-panel") this.renderProcessTable(); if (id === "downloads-panel") this.renderDownloads(); }
//  }
//}
//const unifiedEngine = new UnifiedChromiumEngine();
//</script>
//</body>
//</html>
//
