cat > /mnt/user-data/outputs/gastos-app.html << 'ENDOFFILE'
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<meta name="theme-color" content="#1a1a2e">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="MisGastos">
<title>MisGastos</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap');
:root{--bg:#0f0f1a;--surface:#1a1a2e;--surface2:#252540;--accent:#7c5cfc;--accent2:#fc5c7d;--green:#2ecc71;--red:#e74c3c;--text:#e8e8f0;--muted:#7a7a9a;--border:rgba(124,92,252,0.2);--radius:16px;--font:'Space Grotesk',sans-serif;--mono:'JetBrains Mono',monospace}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font);background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;-webkit-tap-highlight-color:transparent}

/* SPLASH */
#splash{position:fixed;inset:0;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9999;transition:opacity .5s}
#splash.hidden{opacity:0;pointer-events:none}
.splash-logo{font-size:3.5rem;margin-bottom:.5rem}
.splash-title{font-size:1.8rem;font-weight:700;letter-spacing:-1px}
.splash-sub{color:var(--muted);margin-top:.4rem;font-size:.9rem}
.splash-bar{width:200px;height:3px;background:var(--surface2);border-radius:2px;margin-top:2.5rem;overflow:hidden}
.splash-fill{height:100%;width:0;background:linear-gradient(90deg,var(--accent),var(--accent2));border-radius:2px;animation:fill 1.8s ease forwards}
@keyframes fill{to{width:100%}}

/* LOGIN */
#login-screen{position:fixed;inset:0;background:var(--bg);z-index:500;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:2rem}
#login-screen.hidden{display:none}
.login-logo{font-size:3rem;margin-bottom:.5rem}
.login-title{font-size:1.6rem;font-weight:700;letter-spacing:-0.5px;margin-bottom:.3rem}
.login-sub{color:var(--muted);font-size:.85rem;margin-bottom:2rem;text-align:center}
.login-box{width:100%;max-width:360px}
.login-tabs{display:flex;background:var(--surface2);border-radius:10px;padding:3px;margin-bottom:1.5rem}
.login-tab{flex:1;background:none;border:none;color:var(--muted);padding:.55rem;border-radius:8px;font-family:var(--font);font-size:.82rem;font-weight:600;cursor:pointer;transition:all .2s}
.login-tab.active{background:var(--accent);color:#fff}
.users-list{margin-bottom:1.2rem}
.user-item{display:flex;align-items:center;gap:.9rem;padding:.85rem 1rem;background:var(--surface);border:1px solid var(--border);border-radius:12px;margin-bottom:.6rem;cursor:pointer;transition:all .2s}
.user-item:active{border-color:var(--accent);background:rgba(124,92,252,.1)}
.user-avatar{width:40px;height:40px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1.2rem;font-weight:700;color:#fff;flex-shrink:0}
.user-name{font-weight:600;font-size:.95rem}
.user-meta{font-size:.72rem;color:var(--muted)}
.user-item-del{background:none;border:none;color:var(--muted);font-size:1rem;cursor:pointer;padding:.3rem;margin-left:auto}

/* NAV */
nav{position:fixed;bottom:0;left:0;right:0;background:var(--surface);border-top:1px solid var(--border);display:flex;z-index:100;padding-bottom:env(safe-area-inset-bottom)}
nav button{flex:1;background:none;border:none;color:var(--muted);padding:.9rem .5rem .6rem;font-family:var(--font);display:flex;flex-direction:column;align-items:center;gap:3px;cursor:pointer;transition:color .2s;font-size:.65rem;font-weight:500;letter-spacing:.3px;text-transform:uppercase}
nav button .icon{font-size:1.4rem;line-height:1}
nav button.active{color:var(--accent)}
nav button.active .nav-dot{display:block}
.nav-dot{display:none;width:4px;height:4px;background:var(--accent);border-radius:50%;margin-top:2px}

/* MAIN */
main{padding:0 0 calc(70px + env(safe-area-inset-bottom));min-height:100vh}
.page{display:none}
.page.active{display:block}

/* HEADER */
.page-header{padding:3rem 1.2rem 1.5rem;background:linear-gradient(180deg,var(--surface) 0%,transparent 100%)}
.page-header h1{font-size:1.7rem;font-weight:700;letter-spacing:-0.5px}
.page-header p{color:var(--muted);font-size:.85rem;margin-top:.2rem}
.header-user{display:flex;align-items:center;justify-content:space-between}
.header-user-info{display:flex;align-items:center;gap:.75rem}
.header-avatar{width:36px;height:36px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1rem;font-weight:700;color:#fff}
.btn-switch-user{background:var(--surface2);border:1px solid var(--border);color:var(--muted);font-family:var(--font);font-size:.7rem;padding:.35rem .7rem;border-radius:20px;cursor:pointer}

/* CARDS */
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1.2rem;margin:0 1rem 1rem}

/* DASHBOARD */
.balance-card{margin:0 1rem 1rem;background:linear-gradient(135deg,var(--accent) 0%,var(--accent2) 100%);border-radius:var(--radius);padding:1.5rem;position:relative;overflow:hidden}
.balance-card::before{content:'';position:absolute;top:-40px;right:-40px;width:150px;height:150px;background:rgba(255,255,255,.08);border-radius:50%}
.balance-label{font-size:.75rem;text-transform:uppercase;letter-spacing:1px;opacity:.8}
.balance-amount{font-size:2.4rem;font-weight:700;font-family:var(--mono);margin:.3rem 0;letter-spacing:-1px}
.balance-period{font-size:.75rem;opacity:.7}
.stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:.8rem;margin:0 1rem 1rem}
.stat-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1rem}
.stat-icon{font-size:1.3rem;margin-bottom:.4rem}
.stat-label{font-size:.7rem;color:var(--muted);text-transform:uppercase;letter-spacing:.5px}
.stat-value{font-size:1.2rem;font-weight:700;font-family:var(--mono);margin-top:.1rem}

/* CAT CHIPS */
.cat-row{display:flex;gap:.5rem;overflow-x:auto;padding:0 1rem 1rem;scrollbar-width:none}
.cat-row::-webkit-scrollbar{display:none}
.cat-chip{flex-shrink:0;background:var(--surface);border:1px solid var(--border);border-radius:20px;padding:.4rem .9rem;font-size:.78rem;font-family:var(--font);color:var(--muted);cursor:pointer;transition:all .2s;white-space:nowrap}
.cat-chip.active{background:var(--accent);border-color:var(--accent);color:#fff}

/* EXPENSE LIST */
.expense-item{display:flex;align-items:center;gap:.9rem;padding:.85rem 1rem;border-bottom:1px solid var(--border);cursor:pointer;transition:background .15s}
.expense-item:last-child{border-bottom:none}
.expense-item:active{background:var(--surface2)}
.expense-icon{width:42px;height:42px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:1.2rem;flex-shrink:0}
.expense-info{flex:1;min-width:0}
.expense-name{font-weight:600;font-size:.92rem;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.expense-meta{color:var(--muted);font-size:.75rem;margin-top:1px}
.expense-amount{font-family:var(--mono);font-weight:600;font-size:.95rem}
.has-photo{font-size:.6rem;color:var(--accent);margin-left:.3rem}

/* ADD FORM */
.form-group{margin-bottom:1rem}
.form-label{font-size:.78rem;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;margin-bottom:.4rem;display:block}
.form-input{width:100%;background:var(--surface2);border:1px solid var(--border);border-radius:10px;padding:.8rem 1rem;color:var(--text);font-family:var(--font);font-size:.95rem;outline:none;transition:border-color .2s}
.form-input:focus{border-color:var(--accent)}
.form-input::placeholder{color:var(--muted)}
select.form-input{appearance:none;cursor:pointer}
.amount-input{font-family:var(--mono);font-size:1.4rem;font-weight:600;text-align:center}
.cat-selector{display:grid;grid-template-columns:repeat(4,1fr);gap:.5rem;margin-bottom:1rem}
.cat-btn{background:var(--surface2);border:2px solid transparent;border-radius:12px;padding:.7rem .3rem;cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:.25rem;transition:all .2s}
.cat-btn.selected{border-color:var(--accent);background:rgba(124,92,252,.15)}
.cat-btn span:first-child{font-size:1.4rem}
.cat-btn span:last-child{font-size:.6rem;color:var(--muted);font-weight:500}
.cat-btn.selected span:last-child{color:var(--accent)}

/* PHOTO */
.photo-zone{border:2px dashed var(--border);border-radius:var(--radius);padding:1.2rem;text-align:center;cursor:pointer;transition:border-color .2s,background .2s;margin-bottom:1rem}
.photo-zone.has-photo{border-color:var(--accent);background:rgba(124,92,252,.07)}
.photo-zone-icon{font-size:2rem;margin-bottom:.4rem}
.photo-zone-text{font-size:.8rem;color:var(--muted)}
.photo-zone-btns{display:flex;gap:.6rem;justify-content:center;margin-top:.8rem}
.photo-zone-btn{background:var(--surface2);border:1px solid var(--border);color:var(--text);font-family:var(--font);font-size:.75rem;padding:.45rem .9rem;border-radius:20px;cursor:pointer;transition:all .2s}
.photo-zone-btn:active{border-color:var(--accent);color:var(--accent)}
.photo-preview{width:100%;max-height:200px;object-fit:cover;border-radius:10px;display:none;margin-top:.8rem}
.photo-remove{display:none;width:100%;background:none;border:none;color:var(--red);font-family:var(--font);font-size:.75rem;cursor:pointer;margin-top:.4rem;padding:.3rem}

/* BTN */
.btn{width:100%;background:linear-gradient(135deg,var(--accent),var(--accent2));border:none;border-radius:var(--radius);padding:1rem;color:#fff;font-family:var(--font);font-size:1rem;font-weight:600;cursor:pointer;letter-spacing:.3px;transition:opacity .2s,transform .1s}
.btn:active{opacity:.85;transform:scale(.98)}
.btn-ghost{background:var(--surface2);border:1px solid var(--border);color:var(--text);font-size:.88rem;padding:.75rem}
.btn-sm{padding:.6rem 1rem;font-size:.85rem;width:auto;border-radius:10px}
.btn-danger{background:var(--red)}
.btn-outline{background:none;border:1px solid var(--accent);color:var(--accent)}

/* REPORTS */
.period-tabs{display:flex;background:var(--surface2);border-radius:10px;padding:3px;margin:0 1rem 1rem}
.period-tab{flex:1;background:none;border:none;color:var(--muted);padding:.55rem;border-radius:8px;font-family:var(--font);font-size:.78rem;font-weight:600;cursor:pointer;transition:all .2s;text-transform:uppercase;letter-spacing:.3px}
.period-tab.active{background:var(--accent);color:#fff}
.chart-wrap{height:160px;position:relative;margin:.5rem 0 1rem}
.bar-chart{display:flex;align-items:flex-end;gap:.35rem;height:130px}
.bar-col{flex:1;display:flex;flex-direction:column;align-items:center;gap:.3rem}
.bar{width:100%;border-radius:6px 6px 0 0;background:linear-gradient(180deg,var(--accent),rgba(124,92,252,.4));min-height:2px;cursor:pointer}
.bar.highlight{background:linear-gradient(180deg,var(--accent2),rgba(252,92,125,.4))}
.bar-label{font-size:.6rem;color:var(--muted);text-align:center}
.cat-row-item{display:flex;align-items:center;gap:.75rem;padding:.5rem 0}
.cat-row-item+.cat-row-item{border-top:1px solid var(--border)}
.cat-pct-bar{flex:1;height:6px;background:var(--surface2);border-radius:3px;overflow:hidden}
.cat-pct-fill{height:100%;border-radius:3px;background:var(--accent);transition:width .6s}
.cat-row-pct{font-size:.75rem;color:var(--muted);font-family:var(--mono);min-width:35px;text-align:right}
.cat-row-amount{font-size:.8rem;font-family:var(--mono);min-width:65px;text-align:right}

/* MODALS */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:200;display:flex;align-items:flex-end;opacity:0;pointer-events:none;transition:opacity .25s}
.modal-overlay.open{opacity:1;pointer-events:all}
.modal-sheet{background:var(--surface);border-radius:20px 20px 0 0;padding:1.5rem 1.2rem calc(1.5rem + env(safe-area-inset-bottom));width:100%;transform:translateY(100%);transition:transform .3s cubic-bezier(.4,0,.2,1);border-top:1px solid var(--border)}
.modal-overlay.open .modal-sheet{transform:translateY(0)}
.modal-handle{width:36px;height:4px;background:var(--surface2);border-radius:2px;margin:0 auto 1.5rem}
.modal-title{font-size:1.1rem;font-weight:700;margin-bottom:1.2rem}
.share-options{display:flex;gap:.8rem}
.share-btn{flex:1;padding:1rem .5rem;border-radius:14px;border:1px solid var(--border);background:var(--surface2);cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:.4rem;transition:all .2s}
.share-btn:active{transform:scale(.95);border-color:var(--accent)}
.share-btn-icon{font-size:1.8rem}
.share-btn-label{font-size:.7rem;color:var(--muted);font-family:var(--font);font-weight:600}
.detail-photo{width:100%;border-radius:12px;max-height:250px;object-fit:cover;margin-bottom:1rem}
.detail-row{display:flex;justify-content:space-between;align-items:center;padding:.6rem 0;border-bottom:1px solid var(--border)}
.detail-row:last-child{border-bottom:none}
.detail-key{font-size:.8rem;color:var(--muted)}
.detail-val{font-size:.88rem;font-weight:600}

/* EMPTY */
.empty{text-align:center;padding:3rem 2rem;color:var(--muted)}
.empty-icon{font-size:3rem;margin-bottom:.8rem}
.empty-title{font-size:1rem;font-weight:600;color:var(--text);margin-bottom:.3rem}
.empty-sub{font-size:.82rem;line-height:1.5}

/* SETTINGS */
.settings-item{display:flex;align-items:center;justify-content:space-between;padding:.9rem 1rem;border-bottom:1px solid var(--border);cursor:pointer}
.settings-item:last-child{border-bottom:none}
.settings-left{display:flex;align-items:center;gap:.75rem}
.settings-icon{font-size:1.2rem;width:36px;height:36px;background:var(--surface2);border-radius:10px;display:flex;align-items:center;justify-content:center}
.settings-label{font-size:.9rem;font-weight:500}
.settings-sub{font-size:.75rem;color:var(--muted);margin-top:1px}

/* TOAST */
.toast{position:fixed;bottom:calc(75px + env(safe-area-inset-bottom));left:50%;transform:translateX(-50%) translateY(20px);background:var(--surface2);border:1px solid var(--border);border-radius:30px;padding:.6rem 1.2rem;font-size:.82rem;font-weight:500;opacity:0;transition:all .3s;pointer-events:none;white-space:nowrap;z-index:300}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}

/* FAB */
.fab{position:fixed;bottom:calc(80px + env(safe-area-inset-bottom));right:1.2rem;width:52px;height:52px;border-radius:50%;background:linear-gradient(135deg,var(--accent),var(--accent2));border:none;color:#fff;font-size:1.6rem;cursor:pointer;box-shadow:0 4px 20px rgba(124,92,252,.4);display:flex;align-items:center;justify-content:center;transition:transform .2s;z-index:90}
.fab:active{transform:scale(.92)}
.section-title{font-size:.7rem;text-transform:uppercase;letter-spacing:1px;color:var(--muted);padding:.75rem 1rem .3rem;font-weight:600}

/* AVATAR COLORS */
.av0{background:#7c5cfc}.av1{background:#fc5c7d}.av2{background:#2ecc71}.av3{background:#f39c12}.av4{background:#3498db}.av5{background:#e67e22}.av6{background:#9b59b6}.av7{background:#1abc9c}

::-webkit-scrollbar{width:0}
.slide-up{animation:slideUp .3s ease}
@keyframes slideUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
</style>
</head>
<body>

<!-- SPLASH -->
<div id="splash">
  <div class="splash-logo">💰</div>
  <div class="splash-title">MisGastos</div>
  <div class="splash-sub">Control inteligente de tus finanzas</div>
  <div class="splash-bar"><div class="splash-fill"></div></div>
</div>

<!-- LOGIN SCREEN -->
<div id="login-screen">
  <div class="login-logo">💰</div>
  <div class="login-title">MisGastos</div>
  <div class="login-sub">Selecciona tu perfil o crea uno nuevo</div>
  <div class="login-box">
    <div class="login-tabs">
      <button class="login-tab active" id="tab-select">Mis perfiles</button>
      <button class="login-tab" id="tab-new">+ Nuevo usuario</button>
    </div>
    <!-- PANEL: seleccionar usuario -->
    <div id="panel-select">
      <div class="users-list" id="users-list">
        <div class="empty" style="padding:1.5rem">
          <div class="empty-icon">👤</div>
          <div class="empty-title">Sin perfiles</div>
          <div class="empty-sub">Crea tu primer perfil</div>
        </div>
      </div>
    </div>
    <!-- PANEL: nuevo usuario -->
    <div id="panel-new" style="display:none">
      <div class="form-group">
        <label class="form-label">Nombre</label>
        <input type="text" class="form-input" id="new-user-name" placeholder="Tu nombre">
      </div>
      <div class="form-group">
        <label class="form-label">PIN de acceso (4 dígitos)</label>
        <input type="number" class="form-input" id="new-user-pin" placeholder="1234" maxlength="4" inputmode="numeric" style="text-align:center;font-family:var(--mono);font-size:1.4rem;letter-spacing:.5rem">
      </div>
      <div class="form-group">
        <label class="form-label">Moneda</label>
        <select class="form-input" id="new-user-currency">
          <option value="USD">USD $</option><option value="MXN">MXN $</option>
          <option value="COP">COP $</option><option value="EUR">EUR €</option>
          <option value="ARS">ARS $</option><option value="PEN">PEN S/</option>
          <option value="CLP">CLP $</option><option value="GTQ">GTQ Q</option>
          <option value="HNL">HNL L</option><option value="DOP">DOP $</option>
        </select>
      </div>
      <button class="btn" id="btn-create-user">Crear perfil</button>
    </div>
  </div>
</div>

<!-- PIN MODAL -->
<div class="modal-overlay" id="pin-modal">
  <div class="modal-sheet">
    <div class="modal-handle"></div>
    <div class="modal-title" id="pin-modal-title">Ingresa tu PIN</div>
    <div style="text-align:center;margin-bottom:1.2rem">
      <input type="number" class="form-input" id="pin-input" placeholder="••••" maxlength="4" inputmode="numeric" style="text-align:center;font-family:var(--mono);font-size:1.8rem;letter-spacing:.8rem;max-width:180px;margin:0 auto">
    </div>
    <div style="display:flex;gap:.8rem">
      <button class="btn btn-ghost" id="btn-pin-cancel" style="flex:1">Cancelar</button>
      <button class="btn" id="btn-pin-confirm" style="flex:1">Entrar</button>
    </div>
  </div>
</div>

<main>

<!-- HOME -->
<div id="page-home" class="page active">
  <div class="page-header">
    <div class="header-user">
      <div class="header-user-info">
        <div class="header-avatar av0" id="home-avatar">?</div>
        <div>
          <h1 style="font-size:1.3rem" id="home-greeting">Hola!</h1>
          <p id="date-display">Cargando...</p>
        </div>
      </div>
      <button class="btn-switch-user" id="btn-switch">↩ Cambiar</button>
    </div>
  </div>

  <div class="balance-card slide-up">
    <div class="balance-label">Gasto del mes</div>
    <div class="balance-amount" id="month-total">$0.00</div>
    <div class="balance-period" id="month-label">Este mes</div>
  </div>

  <div class="stats-grid slide-up">
    <div class="stat-card"><div class="stat-icon">📅</div><div class="stat-label">Hoy</div><div class="stat-value" id="today-total">$0.00</div></div>
    <div class="stat-card"><div class="stat-icon">📆</div><div class="stat-label">Esta semana</div><div class="stat-value" id="week-total">$0.00</div></div>
    <div class="stat-card"><div class="stat-icon">🧾</div><div class="stat-label">Transacciones</div><div class="stat-value" id="count-total">0</div></div>
    <div class="stat-card"><div class="stat-icon">📷</div><div class="stat-label">Con facturas</div><div class="stat-value" id="photo-count">0</div></div>
  </div>

  <div class="section-title">Gastos recientes</div>
  <div class="card" style="padding:0"><div id="recent-list"><div class="empty"><div class="empty-icon">🌱</div><div class="empty-title">Sin gastos aún</div><div class="empty-sub">Toca + para registrar tu primer gasto</div></div></div></div>
</div>

<!-- ADD -->
<div id="page-add" class="page">
  <div class="page-header"><h1>Nuevo Gasto</h1><p>Registra y adjunta tu factura</p></div>
  <div style="padding:0 1rem">
    <div class="form-group">
      <label class="form-label">Monto</label>
      <input type="number" class="form-input amount-input" id="input-amount" placeholder="0.00" step="0.01" min="0" inputmode="decimal">
    </div>
    <div class="form-group">
      <label class="form-label">Descripción</label>
      <input type="text" class="form-input" id="input-desc" placeholder="¿En qué gastaste?">
    </div>
    <div class="form-group">
      <label class="form-label">Categoría</label>
      <div class="cat-selector" id="cat-selector">
        <button class="cat-btn selected" data-cat="🍔 Comida"><span>🍔</span><span>Comida</span></button>
        <button class="cat-btn" data-cat="🚗 Transporte"><span>🚗</span><span>Transporte</span></button>
        <button class="cat-btn" data-cat="🏠 Hogar"><span>🏠</span><span>Hogar</span></button>
        <button class="cat-btn" data-cat="💊 Salud"><span>💊</span><span>Salud</span></button>
        <button class="cat-btn" data-cat="🎮 Ocio"><span>🎮</span><span>Ocio</span></button>
        <button class="cat-btn" data-cat="👗 Ropa"><span>👗</span><span>Ropa</span></button>
        <button class="cat-btn" data-cat="📚 Educación"><span>📚</span><span>Educación</span></button>
        <button class="cat-btn" data-cat="⚡ Servicios"><span>⚡</span><span>Servicios</span></button>
        <button class="cat-btn" data-cat="🛒 Supermercado"><span>🛒</span><span>Super</span></button>
        <button class="cat-btn" data-cat="✈️ Viajes"><span>✈️</span><span>Viajes</span></button>
        <button class="cat-btn" data-cat="💻 Tecnología"><span>💻</span><span>Tecno</span></button>
        <button class="cat-btn" data-cat="📌 Otro"><span>📌</span><span>Otro</span></button>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">Fecha</label>
      <input type="date" class="form-input" id="input-date">
    </div>
    <div class="form-group">
      <label class="form-label">Nota (opcional)</label>
      <input type="text" class="form-input" id="input-note" placeholder="Notas adicionales...">
    </div>
    <label class="form-label">Foto de factura / recibo</label>
    <div class="photo-zone" id="photo-zone">
      <input type="file" id="photo-input-cam" accept="image/*" capture="environment" style="display:none">
      <input type="file" id="photo-input-gallery" accept="image/*" style="display:none">
      <div id="photo-placeholder">
        <div class="photo-zone-icon">🧾</div>
        <div class="photo-zone-text">Adjunta la factura o recibo</div>
        <div class="photo-zone-btns">
          <button class="photo-zone-btn" id="btn-cam">📷 Cámara</button>
          <button class="photo-zone-btn" id="btn-gallery">🖼️ Galería</button>
        </div>
      </div>
      <img class="photo-preview" id="photo-preview" alt="Factura">
      <button class="photo-remove" id="btn-remove-photo">✕ Quitar foto</button>
    </div>
    <button class="btn" id="btn-save">💾 Guardar Gasto</button>
    <div style="height:1rem"></div>
  </div>
</div>

<!-- HISTORY -->
<div id="page-history" class="page">
  <div class="page-header"><h1>Historial</h1><p>Todos tus gastos registrados</p></div>
  <div class="cat-row" id="filter-cats">
    <div class="cat-chip active" data-filter="all">Todos</div>
    <div class="cat-chip" data-filter="🍔 Comida">🍔 Comida</div>
    <div class="cat-chip" data-filter="🚗 Transporte">🚗 Transporte</div>
    <div class="cat-chip" data-filter="🏠 Hogar">🏠 Hogar</div>
    <div class="cat-chip" data-filter="💊 Salud">💊 Salud</div>
    <div class="cat-chip" data-filter="🎮 Ocio">🎮 Ocio</div>
    <div class="cat-chip" data-filter="👗 Ropa">👗 Ropa</div>
    <div class="cat-chip" data-filter="📚 Educación">📚 Educación</div>
    <div class="cat-chip" data-filter="⚡ Servicios">⚡ Servicios</div>
    <div class="cat-chip" data-filter="🛒 Supermercado">🛒 Super</div>
    <div class="cat-chip" data-filter="✈️ Viajes">✈️ Viajes</div>
    <div class="cat-chip" data-filter="💻 Tecnología">💻 Tecno</div>
    <div class="cat-chip" data-filter="📌 Otro">📌 Otro</div>
  </div>
  <div class="card" style="padding:0"><div id="history-list"></div></div>
</div>

<!-- REPORTS -->
<div id="page-reports" class="page">
  <div class="page-header"><h1>Reportes</h1><p id="report-user-label">Tus finanzas</p></div>
  <div class="period-tabs">
    <button class="period-tab active" data-period="day">Día</button>
    <button class="period-tab" data-period="week">Semana</button>
    <button class="period-tab" data-period="month">Mes</button>
    <button class="period-tab" data-period="year">Año</button>
  </div>
  <div class="card">
    <div class="section-title" style="padding:0 0 .5rem">Evolución de gastos</div>
    <div class="chart-wrap"><div class="bar-chart" id="bar-chart"></div></div>
    <div style="display:flex;justify-content:space-between;align-items:center;margin-top:.5rem">
      <div>
        <div style="font-size:.7rem;color:var(--muted);text-transform:uppercase;letter-spacing:.5px">Total período</div>
        <div style="font-size:1.5rem;font-weight:700;font-family:var(--mono);margin-top:.1rem" id="report-total">$0.00</div>
      </div>
      <div style="text-align:right">
        <div style="font-size:.7rem;color:var(--muted);text-transform:uppercase;letter-spacing:.5px">Promedio</div>
        <div style="font-size:1rem;font-weight:600;font-family:var(--mono);margin-top:.1rem" id="report-avg">$0.00</div>
      </div>
    </div>
  </div>
  <div class="card">
    <div class="section-title" style="padding:0 0 .75rem">Por categoría</div>
    <div id="cat-breakdown"></div>
  </div>
  <div style="padding:0 1rem 1rem">
    <button class="btn" id="btn-share-report">📤 Compartir Reporte</button>
  </div>
</div>

<!-- SETTINGS -->
<div id="page-settings" class="page">
  <div class="page-header"><h1>Configuración</h1><p id="settings-user-label">Tu perfil</p></div>
  <div class="section-title">Perfil</div>
  <div class="card" style="padding:0">
    <div class="settings-item">
      <div class="settings-left">
        <div class="settings-icon">💱</div>
        <div><div class="settings-label">Moneda</div><div class="settings-sub" id="currency-display">USD $</div></div>
      </div>
      <select id="currency-select" style="background:var(--surface2);border:1px solid var(--border);color:var(--text);font-family:var(--font);padding:.4rem .6rem;border-radius:8px;font-size:.82rem">
        <option value="USD">USD $</option><option value="MXN">MXN $</option>
        <option value="COP">COP $</option><option value="EUR">EUR €</option>
        <option value="ARS">ARS $</option><option value="PEN">PEN S/</option>
        <option value="CLP">CLP $</option><option value="GTQ">GTQ Q</option>
        <option value="HNL">HNL L</option><option value="DOP">DOP $</option>
      </select>
    </div>
    <div class="settings-item" id="btn-switch-settings">
      <div class="settings-left">
        <div class="settings-icon">👥</div>
        <div><div class="settings-label">Cambiar usuario</div><div class="settings-sub">Ir a pantalla de perfiles</div></div>
      </div>
      <span style="color:var(--muted)">›</span>
    </div>
  </div>
  <div class="section-title">Email para reportes</div>
  <div class="card">
    <input type="email" class="form-input" id="email-input" placeholder="tu@email.com">
    <div style="height:.8rem"></div>
    <button class="btn btn-ghost" id="btn-save-email">Guardar email</button>
  </div>
  <div class="section-title">Datos</div>
  <div class="card" style="padding:0">
    <div class="settings-item" id="btn-export-csv">
      <div class="settings-left">
        <div class="settings-icon">📊</div>
        <div><div class="settings-label">Exportar CSV</div><div class="settings-sub">Descarga todos tus gastos</div></div>
      </div>
      <span style="color:var(--muted)">›</span>
    </div>
    <div class="settings-item" id="btn-clear">
      <div class="settings-left">
        <div class="settings-icon" style="background:rgba(231,76,60,.1)">🗑️</div>
        <div><div class="settings-label" style="color:var(--red)">Borrar mis gastos</div><div class="settings-sub">Solo borra tus datos</div></div>
      </div>
      <span style="color:var(--muted)">›</span>
    </div>
  </div>
  <div style="text-align:center;padding:2rem 1rem;color:var(--muted);font-size:.75rem">MisGastos v2.0 · Datos guardados localmente</div>
</div>

</main>

<nav>
  <button class="active" data-page="home"><span class="icon">🏠</span><span>Inicio</span><span class="nav-dot"></span></button>
  <button data-page="history"><span class="icon">📋</span><span>Historial</span><span class="nav-dot"></span></button>
  <button data-page="reports"><span class="icon">📊</span><span>Reportes</span><span class="nav-dot"></span></button>
  <button data-page="settings"><span class="icon">⚙️</span><span>Config</span><span class="nav-dot"></span></button>
</nav>

<button class="fab" id="fab">+</button>
<div class="toast" id="toast"></div>

<!-- DETAIL MODAL -->
<div class="modal-overlay" id="detail-modal">
  <div class="modal-sheet">
    <div class="modal-handle"></div>
    <div class="modal-title" id="detail-title">Detalle del gasto</div>
    <img class="detail-photo" id="detail-photo" alt="Factura">
    <div id="detail-rows"></div>
    <div style="height:1rem"></div>
    <div style="display:flex;gap:.8rem">
      <button class="btn btn-ghost" style="flex:1" id="btn-close-detail">Cerrar</button>
      <button class="btn btn-danger" style="flex:1" id="btn-delete-expense">🗑️ Eliminar</button>
    </div>
  </div>
</div>

<!-- SHARE MODAL -->
<div class="modal-overlay" id="share-modal">
  <div class="modal-sheet">
    <div class="modal-handle"></div>
    <div class="modal-title">Compartir reporte</div>
    <div class="share-options">
      <button class="share-btn" id="share-email"><div class="share-btn-icon">📧</div><div class="share-btn-label">Email</div></button>
      <button class="share-btn" id="share-whatsapp"><div class="share-btn-icon">💬</div><div class="share-btn-label">WhatsApp</div></button>
      <button class="share-btn" id="share-telegram"><div class="share-btn-icon">✈️</div><div class="share-btn-label">Telegram</div></button>
      <button class="share-btn" id="share-copy"><div class="share-btn-icon">📋</div><div class="share-btn-label">Copiar</div></button>
    </div>
    <div style="height:1rem"></div>
    <button class="btn btn-ghost" id="btn-close-share">Cancelar</button>
  </div>
</div>

<script>
// ══════════════════════════════════════════
//  CONSTANTS & STATE
// ══════════════════════════════════════════
const CURRENCIES = {USD:'$',MXN:'$',COP:'$',EUR:'€',ARS:'$',PEN:'S/',CLP:'$',GTQ:'Q',HNL:'L',DOP:'$'};
const CAT_COLORS = {'🍔':'#ff6b6b','🚗':'#48dbfb','🏠':'#ff9ff3','💊':'#54a0ff','🎮':'#5f27cd','👗':'#ff6b81','📚':'#feca57','⚡':'#01CBC6','🛒':'#ff9f43','✈️':'#00d2d3','💻':'#341f97','📌':'#636e72'};
const CHART_COLORS = ['#7c5cfc','#fc5c7d','#2ecc71','#f39c12','#3498db','#e67e22','#9b59b6','#1abc9c'];

// All users stored as array
// Each user: { id, name, pin, currency, email, colorIdx, expenses:[] }
let users = JSON.parse(localStorage.getItem('mg_users') || '[]');
let currentUser = null;  // reference into users[]
let pendingLoginUserId = null;
let currentFilter = 'all';
let currentPeriod = 'month';
let currentDetailId = null;
let selectedCat = '🍔 Comida';
let photoData = null;

function saveAll() { localStorage.setItem('mg_users', JSON.stringify(users)); }
function genId() { return Date.now().toString(36) + Math.random().toString(36).slice(2); }

function fmt(n) {
  const sym = CURRENCIES[currentUser?.currency || 'USD'] || '$';
  return sym + parseFloat(n||0).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}

function getExpenses() { return currentUser ? currentUser.expenses : []; }

// ══════════════════════════════════════════
//  SPLASH
// ══════════════════════════════════════════
setTimeout(() => document.getElementById('splash').classList.add('hidden'), 2000);

// ══════════════════════════════════════════
//  LOGIN SYSTEM
// ══════════════════════════════════════════
function showLogin() {
  document.getElementById('login-screen').classList.remove('hidden');
  document.querySelector('nav').style.display = 'none';
  document.getElementById('fab').style.display = 'none';
  renderUsersList();
}

function hideLogin() {
  document.getElementById('login-screen').classList.add('hidden');
  document.querySelector('nav').style.display = 'flex';
  document.getElementById('fab').style.display = 'flex';
}

function renderUsersList() {
  const el = document.getElementById('users-list');
  if (!users.length) {
    el.innerHTML = `<div class="empty" style="padding:1.5rem"><div class="empty-icon">👤</div><div class="empty-title">Sin perfiles</div><div class="empty-sub">Crea tu primer perfil usando la pestaña de arriba</div></div>`;
    return;
  }
  el.innerHTML = users.map(u => `
    <div class="user-item" data-uid="${u.id}">
      <div class="user-avatar av${u.colorIdx%8}">${u.name.charAt(0).toUpperCase()}</div>
      <div>
        <div class="user-name">${u.name}</div>
        <div class="user-meta">${u.expenses.length} gastos · ${u.currency}</div>
      </div>
      <button class="user-item-del" data-del="${u.id}" title="Eliminar usuario">🗑️</button>
    </div>
  `).join('');

  el.querySelectorAll('.user-item').forEach(item => {
    item.addEventListener('click', e => {
      if (e.target.dataset.del) return; // handled below
      const uid = item.dataset.uid;
      const user = users.find(u => u.id === uid);
      if (!user) return;
      if (user.pin) { openPinModal(uid); }
      else { loginUser(uid); }
    });
  });
  el.querySelectorAll('[data-del]').forEach(btn => {
    btn.addEventListener('click', e => {
      e.stopPropagation();
      const uid = btn.dataset.del;
      const user = users.find(u => u.id === uid);
      if (!confirm(`¿Eliminar el perfil de "${user?.name}"? Se borrarán todos sus gastos.`)) return;
      users = users.filter(u => u.id !== uid);
      saveAll();
      renderUsersList();
      showToast('🗑️ Perfil eliminado');
    });
  });
}

function openPinModal(uid) {
  pendingLoginUserId = uid;
  const user = users.find(u => u.id === uid);
  document.getElementById('pin-modal-title').textContent = `PIN de ${user.name}`;
  document.getElementById('pin-input').value = '';
  document.getElementById('pin-modal').classList.add('open');
  setTimeout(() => document.getElementById('pin-input').focus(), 300);
}

document.getElementById('btn-pin-cancel').addEventListener('click', () => {
  document.getElementById('pin-modal').classList.remove('open');
  pendingLoginUserId = null;
});

document.getElementById('btn-pin-confirm').addEventListener('click', () => {
  const entered = document.getElementById('pin-input').value.toString().trim();
  const user = users.find(u => u.id === pendingLoginUserId);
  if (!user) return;
  if (entered === user.pin.toString()) {
    document.getElementById('pin-modal').classList.remove('open');
    loginUser(pendingLoginUserId);
  } else {
    showToast('❌ PIN incorrecto');
    document.getElementById('pin-input').value = '';
  }
});

document.getElementById('pin-input').addEventListener('keydown', e => {
  if (e.key === 'Enter') document.getElementById('btn-pin-confirm').click();
});

function loginUser(uid) {
  currentUser = users.find(u => u.id === uid);
  if (!currentUser) return;
  hideLogin();
  updateSettingsUI();
  renderHome();
  goPage('home');
  showToast(`👋 Bienvenido, ${currentUser.name}!`);
}

// New user creation
document.getElementById('tab-select').addEventListener('click', () => {
  document.getElementById('tab-select').classList.add('active');
  document.getElementById('tab-new').classList.remove('active');
  document.getElementById('panel-select').style.display = '';
  document.getElementById('panel-new').style.display = 'none';
});
document.getElementById('tab-new').addEventListener('click', () => {
  document.getElementById('tab-new').classList.add('active');
  document.getElementById('tab-select').classList.remove('active');
  document.getElementById('panel-new').style.display = '';
  document.getElementById('panel-select').style.display = 'none';
});

document.getElementById('btn-create-user').addEventListener('click', () => {
  const name = document.getElementById('new-user-name').value.trim();
  const pin = document.getElementById('new-user-pin').value.toString().trim();
  const currency = document.getElementById('new-user-currency').value;
  if (!name) return showToast('⚠️ Ingresa un nombre');
  if (!pin || pin.length < 4) return showToast('⚠️ El PIN debe tener 4 dígitos');

  const newUser = { id: genId(), name, pin, currency, email: '', colorIdx: users.length, expenses: [] };
  users.push(newUser);
  saveAll();
  showToast(`✅ Perfil "${name}" creado`);
  document.getElementById('new-user-name').value = '';
  document.getElementById('new-user-pin').value = '';
  // switch to select panel
  document.getElementById('tab-select').click();
  renderUsersList();
});

// Switch user buttons
function doSwitchUser() {
  currentUser = null;
  currentFilter = 'all';
  currentPeriod = 'month';
  showLogin();
}
document.getElementById('btn-switch').addEventListener('click', doSwitchUser);
document.getElementById('btn-switch-settings').addEventListener('click', doSwitchUser);

// ══════════════════════════════════════════
//  NAVIGATION
// ══════════════════════════════════════════
function goPage(name) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
  document.getElementById('page-'+name).classList.add('active');
  document.querySelector(`nav button[data-page="${name}"]`).classList.add('active');
  document.getElementById('fab').style.display = name === 'add' ? 'none' : 'flex';
  if (name === 'home') renderHome();
  if (name === 'history') renderHistory();
  if (name === 'reports') renderReports();
}

document.querySelectorAll('nav button').forEach(btn => {
  btn.addEventListener('click', () => { if (currentUser) goPage(btn.dataset.page); });
});
document.getElementById('fab').addEventListener('click', () => { if (currentUser) goPage('add'); });

// ══════════════════════════════════════════
//  HOME
// ══════════════════════════════════════════
function renderHome() {
  if (!currentUser) return;
  const now = new Date();
  document.getElementById('date-display').textContent =
    now.toLocaleDateString('es',{weekday:'long',day:'numeric',month:'long'});
  document.getElementById('home-greeting').textContent = 'Hola, '+currentUser.name+' 👋';
  const av = document.getElementById('home-avatar');
  av.textContent = currentUser.name.charAt(0).toUpperCase();
  av.className = 'header-avatar av'+(currentUser.colorIdx%8);

  const todayStr = now.toISOString().slice(0,10);
  const weekStart = new Date(now); weekStart.setDate(now.getDate() - now.getDay()); weekStart.setHours(0,0,0,0);

  let todayT=0, weekT=0, monthT=0, photoC=0;
  getExpenses().forEach(e => {
    const d = new Date(e.date+'T00:00:00');
    const amt = parseFloat(e.amount)||0;
    if (e.date === todayStr) todayT += amt;
    if (d >= weekStart) weekT += amt;
    if (d.getMonth()===now.getMonth() && d.getFullYear()===now.getFullYear()) monthT += amt;
    if (e.photo) photoC++;
  });

  document.getElementById('month-total').textContent = fmt(monthT);
  document.getElementById('month-label').textContent = now.toLocaleDateString('es',{month:'long',year:'numeric'});
  document.getElementById('today-total').textContent = fmt(todayT);
  document.getElementById('week-total').textContent = fmt(weekT);
  document.getElementById('count-total').textContent = getExpenses().length;
  document.getElementById('photo-count').textContent = photoC;

  const recent = [...getExpenses()].sort((a,b)=>new Date(b.date)-new Date(a.date)).slice(0,5);
  const el = document.getElementById('recent-list');
  if (!recent.length) {
    el.innerHTML = `<div class="empty"><div class="empty-icon">🌱</div><div class="empty-title">Sin gastos aún</div><div class="empty-sub">Toca + para registrar tu primer gasto</div></div>`;
    return;
  }
  el.innerHTML = recent.map(e => expenseHTML(e)).join('');
  el.querySelectorAll('.expense-item').forEach(item =>
    item.addEventListener('click', () => showDetail(item.dataset.id)));
}

function expenseHTML(e) {
  const cat = e.category||'📌 Otro';
  const emoji = cat.split(' ')[0];
  const bg = CAT_COLORS[emoji]||'#636e72';
  const d = new Date(e.date+'T00:00:00').toLocaleDateString('es',{day:'numeric',month:'short'});
  return `<div class="expense-item" data-id="${e.id}">
    <div class="expense-icon" style="background:${bg}22;color:${bg}">${emoji}</div>
    <div class="expense-info">
      <div class="expense-name">${e.desc||cat}${e.photo?'<span class="has-photo">📷</span>':''}</div>
      <div class="expense-meta">${cat} · ${d}</div>
    </div>
    <div class="expense-amount">${fmt(e.amount)}</div>
  </div>`;
}

// ══════════════════════════════════════════
//  ADD EXPENSE
// ══════════════════════════════════════════
document.querySelectorAll('.cat-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('selected'));
    btn.classList.add('selected');
    selectedCat = btn.dataset.cat;
  });
});

// Photo: camera
document.getElementById('btn-cam').addEventListener('click', e => {
  e.stopPropagation();
  document.getElementById('photo-input-cam').click();
});
// Photo: gallery
document.getElementById('btn-gallery').addEventListener('click', e => {
  e.stopPropagation();
  document.getElementById('photo-input-gallery').click();
});

function handlePhotoFile(file) {
  if (!file) return;
  const reader = new FileReader();
  reader.onload = r => {
    photoData = r.result;
    document.getElementById('photo-preview').src = photoData;
    document.getElementById('photo-preview').style.display = 'block';
    document.getElementById('photo-placeholder').style.display = 'none';
    document.getElementById('btn-remove-photo').style.display = 'block';
    document.getElementById('photo-zone').classList.add('has-photo');
  };
  reader.readAsDataURL(file);
}

document.getElementById('photo-input-cam').addEventListener('change', e => handlePhotoFile(e.target.files[0]));
document.getElementById('photo-input-gallery').addEventListener('change', e => handlePhotoFile(e.target.files[0]));

document.getElementById('btn-remove-photo').addEventListener('click', e => {
  e.stopPropagation();
  resetPhoto();
});

function resetPhoto() {
  photoData = null;
  document.getElementById('photo-preview').style.display = 'none';
  document.getElementById('photo-preview').src = '';
  document.getElementById('photo-placeholder').style.display = '';
  document.getElementById('btn-remove-photo').style.display = 'none';
  document.getElementById('photo-zone').classList.remove('has-photo');
  document.getElementById('photo-input-cam').value = '';
  document.getElementById('photo-input-gallery').value = '';
}

document.getElementById('input-date').value = new Date().toISOString().slice(0,10);

document.getElementById('btn-save').addEventListener('click', () => {
  if (!currentUser) return;
  const amount = parseFloat(document.getElementById('input-amount').value);
  const desc = document.getElementById('input-desc').value.trim();
  const date = document.getElementById('input-date').value;
  const note = document.getElementById('input-note').value.trim();

  if (!amount || amount <= 0) return showToast('⚠️ Ingresa un monto válido');
  if (!date) return showToast('⚠️ Selecciona una fecha');

  const expense = { id:genId(), amount, desc, date, note, category:selectedCat, photo:photoData||null };
  currentUser.expenses.unshift(expense);
  saveAll();

  // reset form
  document.getElementById('input-amount').value = '';
  document.getElementById('input-desc').value = '';
  document.getElementById('input-note').value = '';
  document.getElementById('input-date').value = new Date().toISOString().slice(0,10);
  resetPhoto();
  document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('selected'));
  document.querySelector('.cat-btn[data-cat="🍔 Comida"]').classList.add('selected');
  selectedCat = '🍔 Comida';

  showToast('✅ Gasto guardado');
  goPage('home');
});

// ══════════════════════════════════════════
//  HISTORY
// ══════════════════════════════════════════
function renderHistory() {
  if (!currentUser) return;
  const filtered = currentFilter === 'all'
    ? getExpenses()
    : getExpenses().filter(e => e.category === currentFilter);
  const sorted = [...filtered].sort((a,b) => new Date(b.date)-new Date(a.date));

  const el = document.getElementById('history-list');
  if (!sorted.length) {
    el.innerHTML = `<div class="empty"><div class="empty-icon">📭</div><div class="empty-title">Sin gastos</div><div class="empty-sub">No hay gastos en esta categoría</div></div>`;
    return;
  }

  const groups = {};
  sorted.forEach(e => { if (!groups[e.date]) groups[e.date]=[]; groups[e.date].push(e); });

  let html = '';
  Object.keys(groups).sort((a,b)=>new Date(b)-new Date(a)).forEach(date => {
    const d = new Date(date+'T00:00:00');
    const label = d.toLocaleDateString('es',{weekday:'long',day:'numeric',month:'long'});
    const total = groups[date].reduce((s,e)=>s+parseFloat(e.amount||0),0);
    html += `<div style="display:flex;justify-content:space-between;align-items:center;padding:.6rem 1rem .2rem;border-bottom:1px solid var(--border)">
      <span style="font-size:.72rem;color:var(--muted);text-transform:capitalize;font-weight:600">${label}</span>
      <span style="font-size:.72rem;font-family:var(--mono);color:var(--accent)">${fmt(total)}</span>
    </div>`;
    html += groups[date].map(e => expenseHTML(e)).join('');
  });

  el.innerHTML = html;
  el.querySelectorAll('.expense-item').forEach(item =>
    item.addEventListener('click', () => showDetail(item.dataset.id)));
}

document.querySelectorAll('.cat-chip').forEach(chip => {
  chip.addEventListener('click', () => {
    document.querySelectorAll('.cat-chip').forEach(c => c.classList.remove('active'));
    chip.classList.add('active');
    currentFilter = chip.dataset.filter;
    renderHistory();
  });
});

// ══════════════════════════════════════════
//  DETAIL MODAL
// ══════════════════════════════════════════
function showDetail(id) {
  const e = getExpenses().find(x => x.id === id);
  if (!e) return;
  currentDetailId = id;
  document.getElementById('detail-title').textContent = e.desc || e.category;
  const photo = document.getElementById('detail-photo');
  if (e.photo) { photo.src = e.photo; photo.style.display = 'block'; }
  else photo.style.display = 'none';

  const d = new Date(e.date+'T00:00:00').toLocaleDateString('es',{weekday:'long',day:'numeric',month:'long',year:'numeric'});
  document.getElementById('detail-rows').innerHTML = `
    <div class="detail-row"><div class="detail-key">Monto</div><div class="detail-val">${fmt(e.amount)}</div></div>
    <div class="detail-row"><div class="detail-key">Categoría</div><div class="detail-val">${e.category}</div></div>
    <div class="detail-row"><div class="detail-key">Fecha</div><div class="detail-val">${d}</div></div>
    ${e.note?`<div class="detail-row"><div class="detail-key">Nota</div><div class="detail-val">${e.note}</div></div>`:''}
    <div class="detail-row"><div class="detail-key">Factura</div><div class="detail-val">${e.photo?'📷 Adjunta':'—'}</div></div>
  `;
  document.getElementById('detail-modal').classList.add('open');
}

document.getElementById('btn-close-detail').addEventListener('click', () =>
  document.getElementById('detail-modal').classList.remove('open'));
document.getElementById('detail-modal').addEventListener('click', e => {
  if (e.target === document.getElementById('detail-modal'))
    document.getElementById('detail-modal').classList.remove('open');
});
document.getElementById('btn-delete-expense').addEventListener('click', () => {
  if (!confirm('¿Eliminar este gasto?')) return;
  currentUser.expenses = currentUser.expenses.filter(e => e.id !== currentDetailId);
  saveAll();
  document.getElementById('detail-modal').classList.remove('open');
  showToast('🗑️ Gasto eliminado');
  renderHome();
  renderHistory();
});

// ══════════════════════════════════════════
//  REPORTS  ← FIXED
// ══════════════════════════════════════════
function getReportData() {
  if (!currentUser) return { bars:[], filtered:[] };
  const now = new Date();
  const expenses = getExpenses();
  let bars = [];
  let filtered = [];

  if (currentPeriod === 'day') {
    // Last 7 days, one bar per day
    bars = Array.from({length:7}, (_,i) => {
      const d = new Date(now); d.setDate(now.getDate()-6+i); d.setHours(0,0,0,0);
      const iso = d.toISOString().slice(0,10);
      const dayExp = expenses.filter(e => e.date === iso);
      const total = dayExp.reduce((s,e)=>s+parseFloat(e.amount||0),0);
      const isToday = iso === now.toISOString().slice(0,10);
      return { label: d.toLocaleDateString('es',{weekday:'short'}), total, isToday };
    });
    // filtered = today
    const todayStr = now.toISOString().slice(0,10);
    filtered = expenses.filter(e => e.date === todayStr);

  } else if (currentPeriod === 'week') {
    // Current week Mon-Sun, bar per day
    const dayOfWeek = now.getDay() === 0 ? 6 : now.getDay()-1; // Mon=0
    bars = Array.from({length:7}, (_,i) => {
      const d = new Date(now); d.setDate(now.getDate()-dayOfWeek+i); d.setHours(0,0,0,0);
      const iso = d.toISOString().slice(0,10);
      const total = expenses.filter(e=>e.date===iso).reduce((s,e)=>s+parseFloat(e.amount||0),0);
      const isToday = iso === now.toISOString().slice(0,10);
      return { label: d.toLocaleDateString('es',{weekday:'short'}), total, isToday };
    });
    // filtered = this week
    const weekDates = bars.map((_,i) => { const d=new Date(now); d.setDate(now.getDate()-dayOfWeek+i); return d.toISOString().slice(0,10); });
    filtered = expenses.filter(e => weekDates.includes(e.date));

  } else if (currentPeriod === 'month') {
    // Current month, bar per week
    const yr = now.getFullYear(), mo = now.getMonth();
    const daysInMonth = new Date(yr, mo+1, 0).getDate();
    const weeks = Math.ceil(daysInMonth/7);
    bars = Array.from({length:weeks}, (_,i) => {
      const wStart = i*7+1, wEnd = Math.min((i+1)*7, daysInMonth);
      const total = expenses.filter(e => {
        const d = new Date(e.date+'T00:00:00');
        return d.getFullYear()===yr && d.getMonth()===mo && d.getDate()>=wStart && d.getDate()<=wEnd;
      }).reduce((s,e)=>s+parseFloat(e.amount||0),0);
      return { label:`S${i+1}`, total };
    });
    filtered = expenses.filter(e => {
      const d = new Date(e.date+'T00:00:00');
      return d.getFullYear()===yr && d.getMonth()===mo;
    });

  } else { // year
    const yr = now.getFullYear();
    bars = Array.from({length:12}, (_,i) => {
      const label = new Date(yr,i,1).toLocaleDateString('es',{month:'short'});
      const total = expenses.filter(e => {
        const d = new Date(e.date+'T00:00:00');
        return d.getFullYear()===yr && d.getMonth()===i;
      }).reduce((s,e)=>s+parseFloat(e.amount||0),0);
      const isToday = i === now.getMonth();
      return { label, total, isToday };
    });
    filtered = expenses.filter(e => {
      const d = new Date(e.date+'T00:00:00');
      return d.getFullYear()===yr;
    });
  }

  return { bars, filtered };
}

function renderReports() {
  if (!currentUser) return;
  document.getElementById('report-user-label').textContent = 'Finanzas de '+currentUser.name;

  const { bars, filtered } = getReportData();
  const total = filtered.reduce((s,e)=>s+parseFloat(e.amount||0),0);
  const barsWithData = bars.filter(b=>b.total>0);
  const avg = barsWithData.length ? total/barsWithData.length : 0;
  const maxVal = Math.max(...bars.map(b=>b.total), 0.01);

  document.getElementById('report-total').textContent = fmt(total);
  document.getElementById('report-avg').textContent = fmt(avg);

  // Bar chart
  const chartEl = document.getElementById('bar-chart');
  chartEl.innerHTML = bars.map(b => {
    const h = maxVal > 0 ? Math.max((b.total/maxVal)*110, b.total>0?6:1) : 1;
    return `<div class="bar-col">
      <div class="bar${b.isToday?' highlight':''}" style="height:${h}px" title="${fmt(b.total)}"></div>
      <div class="bar-label">${b.label}</div>
    </div>`;
  }).join('');

  // Category breakdown
  const catTotals = {};
  filtered.forEach(e => {
    const c = e.category||'📌 Otro';
    catTotals[c] = (catTotals[c]||0) + parseFloat(e.amount||0);
  });
  const sorted = Object.entries(catTotals).sort((a,b)=>b[1]-a[1]);

  const el = document.getElementById('cat-breakdown');
  if (!sorted.length) {
    el.innerHTML = `<div class="empty-sub" style="padding:.5rem 0;color:var(--muted);text-align:center">Sin gastos en este período</div>`;
    return;
  }
  el.innerHTML = sorted.map(([cat,amt],i) => `
    <div class="cat-row-item">
      <div style="font-size:1.1rem;width:24px">${cat.split(' ')[0]}</div>
      <div style="flex:1;min-width:0">
        <div style="font-size:.8rem;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${cat}</div>
        <div class="cat-pct-bar" style="margin-top:.3rem">
          <div class="cat-pct-fill" style="width:${total>0?(amt/total*100):0}%;background:${CHART_COLORS[i%CHART_COLORS.length]}"></div>
        </div>
      </div>
      <div style="text-align:right">
        <div class="cat-row-amount">${fmt(amt)}</div>
        <div class="cat-row-pct">${total>0?Math.round(amt/total*100):0}%</div>
      </div>
    </div>
  `).join('');
}

document.querySelectorAll('.period-tab').forEach(tab => {
  tab.addEventListener('click', () => {
    document.querySelectorAll('.period-tab').forEach(t => t.classList.remove('active'));
    tab.classList.add('active');
    currentPeriod = tab.dataset.period;
    renderReports();
  });
});

// ══════════════════════════════════════════
//  SHARE / REPORTS
// ══════════════════════════════════════════
function buildReportText() {
  if (!currentUser) return '';
  const { filtered } = getReportData();
  const total = filtered.reduce((s,e)=>s+parseFloat(e.amount||0),0);
  const now = new Date();
  const periodNames = {day:'Hoy',week:'Esta semana',month:'Este mes',year:'Este año'};

  const catTotals = {};
  filtered.forEach(e => {
    const c = e.category||'📌 Otro';
    catTotals[c] = (catTotals[c]||0) + parseFloat(e.amount||0);
  });

  const breakdown = Object.entries(catTotals).sort((a,b)=>b[1]-a[1])
    .map(([c,a]) => `  ${c}: ${fmt(a)} (${total>0?Math.round(a/total*100):0}%)`).join('\n');

  const detail = filtered.sort((a,b)=>new Date(b.date)-new Date(a.date)).slice(0,20)
    .map(e => `  • ${e.date} | ${e.category} | ${e.desc||'-'} | ${fmt(e.amount)}`).join('\n');

  return `💰 REPORTE DE GASTOS — ${periodNames[currentPeriod]}
👤 Usuario: ${currentUser.name}
📅 Generado: ${now.toLocaleDateString('es',{day:'numeric',month:'long',year:'numeric'})}

💵 TOTAL: ${fmt(total)}
📊 Transacciones: ${filtered.length}

📂 POR CATEGORÍA:
${breakdown||'  Sin datos'}

📋 DETALLE DE GASTOS:
${detail||'  Sin gastos en este período'}

—
Generado con MisGastos`;
}

document.getElementById('btn-share-report').addEventListener('click', () => {
  document.getElementById('share-modal').classList.add('open');
});
document.getElementById('btn-close-share').addEventListener('click', () =>
  document.getElementById('share-modal').classList.remove('open'));
document.getElementById('share-modal').addEventListener('click', e => {
  if (e.target === document.getElementById('share-modal'))
    document.getElementById('share-modal').classList.remove('open');
});

document.getElementById('share-email').addEventListener('click', () => {
  const text = buildReportText();
  const email = currentUser?.email || '';
  const subject = encodeURIComponent(`💰 Reporte de Gastos (${currentUser?.name}) — MisGastos`);
  const body = encodeURIComponent(text);
  window.open(`mailto:${email}?subject=${subject}&body=${body}`, '_blank');
  document.getElementById('share-modal').classList.remove('open');
});

document.getElementById('share-whatsapp').addEventListener('click', () => {
  const text = encodeURIComponent(buildReportText());
  window.open(`https://wa.me/?text=${text}`, '_blank');
  document.getElementById('share-modal').classList.remove('open');
});

document.getElementById('share-telegram').addEventListener('click', () => {
  const text = encodeURIComponent(buildReportText());
  window.open(`https://t.me/share/url?url=.&text=${text}`, '_blank');
  document.getElementById('share-modal').classList.remove('open');
});

document.getElementById('share-copy').addEventListener('click', async () => {
  try {
    await navigator.clipboard.writeText(buildReportText());
    showToast('📋 Reporte copiado');
  } catch(e) {
    // fallback
    const ta = document.createElement('textarea');
    ta.value = buildReportText();
    document.body.appendChild(ta); ta.select(); document.execCommand('copy'); ta.remove();
    showToast('📋 Reporte copiado');
  }
  document.getElementById('share-modal').classList.remove('open');
});

// ══════════════════════════════════════════
//  SETTINGS
// ══════════════════════════════════════════
function updateSettingsUI() {
  if (!currentUser) return;
  document.getElementById('settings-user-label').textContent = 'Perfil de '+currentUser.name;
  document.getElementById('currency-select').value = currentUser.currency;
  document.getElementById('currency-display').textContent = currentUser.currency+' '+(CURRENCIES[currentUser.currency]||'$');
  document.getElementById('email-input').value = currentUser.email||'';
}

document.getElementById('currency-select').addEventListener('change', e => {
  if (!currentUser) return;
  currentUser.currency = e.target.value;
  document.getElementById('currency-display').textContent = currentUser.currency+' '+(CURRENCIES[currentUser.currency]||'$');
  saveAll();
  showToast('✅ Moneda actualizada');
  renderHome();
});

document.getElementById('btn-save-email').addEventListener('click', () => {
  if (!currentUser) return;
  currentUser.email = document.getElementById('email-input').value.trim();
  saveAll();
  showToast('✅ Email guardado');
});

document.getElementById('btn-export-csv').addEventListener('click', () => {
  if (!currentUser) return;
  let csv = 'Fecha,Descripción,Categoría,Monto,Nota,Tiene Factura\n';
  getExpenses().forEach(e => {
    csv += `"${e.date}","${e.desc||''}","${e.category}","${e.amount}","${e.note||''}","${e.photo?'Sí':'No'}"\n`;
  });
  const a = document.createElement('a');
  a.href = 'data:text/csv;charset=utf-8,\uFEFF'+encodeURIComponent(csv);
  a.download = `gastos-${currentUser.name}-${new Date().toISOString().slice(0,10)}.csv`;
  a.click();
  showToast('📊 CSV exportado');
});

document.getElementById('btn-clear').addEventListener('click', () => {
  if (!currentUser) return;
  if (!confirm(`¿Borrar TODOS los gastos de "${currentUser.name}"? No se puede deshacer.`)) return;
  currentUser.expenses = [];
  saveAll();
  showToast('🗑️ Gastos eliminados');
  renderHome();
});

// ══════════════════════════════════════════
//  TOAST
// ══════════════════════════════════════════
let toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(()=>t.classList.remove('show'), 2500);
}

// ══════════════════════════════════════════
//  INIT
// ══════════════════════════════════════════
setTimeout(() => {
  if (users.length === 0) {
    showLogin();
  } else {
    showLogin(); // always show login for security
  }
}, 2100);
</script>
</body>
</html>
ENDOFFILE
echo "Done"
Output

Done
Done

Claude Fable 5 is currently unavailable.
