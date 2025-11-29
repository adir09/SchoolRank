<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>SchoolRank – מערכת משוב למורים</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --bg: #020617;
      --bg-alt: #020617;
      --card-bg: #02081a;
      --card-alt: #071427;
      --green-dark: #059669;
      --green-light: #10b981;
      --red: #ef4444;
      --orange: #f59e0b;
      --blue: #3b82f6;
      --purple: #8b5cf6;
      --text-main: #f9fafb;
      --text-muted: #9ca3af;
      --border-soft: #1f2933;
      --shadow-soft: 0 6px 16px rgba(0,0,0,0.55);
      --shadow-hover: 0 18px 40px rgba(0,0,0,0.8);
      --radius-card: 18px;
      --radius-small: 10px;
      --transition-fast: .18s ease-out;
      --transition-smooth: .30s cubic-bezier(0.4, 0, 0.2, 1);
    }

    *{box-sizing:border-box;margin:0;padding:0}

    body{
      font-family:'Segoe UI',system-ui,-apple-system,sans-serif;
      background:radial-gradient(circle at top,#0f172a 0,#020617 55%,#000 100%);
      color:var(--text-main);
      min-height:100vh;
    }

    .app{
      max-width:1200px;
      margin:0 auto;
      min-height:100vh;
      display:flex;
      flex-direction:column;
      background:rgba(2,6,23,.92);
    }

    /* HEADER */

    .app-header{
      background:linear-gradient(90deg,#02081a 0,#0b456a 55%,#0ea5e9 100%);
      color:#fff;
      padding:10px 22px;
      display:flex;
      align-items:center;
      justify-content:space-between;
      border-bottom:1px solid rgba(255,255,255,.1);
      position:sticky;
      top:0;
      z-index:100;
      box-shadow:0 4px 18px rgba(0,0,0,.6);
    }
    .app-header-left{display:flex;align-items:center;gap:10px}
    .logo{
      width:36px;height:36px;border-radius:12px;
      background:radial-gradient(circle at 20% 20%,#22c55e 0,#16a34a 40%,#0f766e 100%);
      display:flex;align-items:center;justify-content:center;
      font-size:18px;
    }
    .app-title{
      font-size:22px;font-weight:800;
      background:linear-gradient(90deg,#e0f2fe,#38bdf8);
      -webkit-background-clip:text;
      -webkit-text-fill-color:transparent;
    }
    .brand-small{
      font-size:13px;
      color:#bae6fd;
    }
    .app-header-right{display:flex;gap:10px;align-items:center}

    .icon-button{
      width:40px;height:40px;border-radius:999px;
      border:1px solid rgba(255,255,255,.18);
      background:rgba(15,23,42,.7);
      display:inline-flex;align-items:center;justify-content:center;
      cursor:pointer;font-size:16px;
      transition:all var(--transition-fast);
      position:relative;
      color:#e5e7eb;
    }
    .icon-button:hover{
      background:rgba(30,64,175,.85);
      transform:translateY(-1px) scale(1.02);
      box-shadow:0 6px 20px rgba(15,23,42,.8);
    }
    .notif-badge{
      position:absolute;top:-4px;right:-4px;
      min-width:18px;height:18px;border-radius:999px;
      background:var(--red);color:#fff;
      font-size:10px;display:flex;align-items:center;justify-content:center;
      padding:0 4px;
    }
    .user-chip{
      font-size:13px;padding:8px 14px;border-radius:999px;
      background:#0b456a;
      border:1px solid rgba(255,255,255,.18);
      display:flex;align-items:center;gap:6px;
      cursor:pointer;
      transition:all var(--transition-fast);
    }
    .user-chip:hover{
      background:#0369a1;
      transform:translateY(-1px);
      box-shadow:0 5px 18px rgba(15,23,42,.8);
    }

    /* MAIN */

    .app-main{padding:22px;flex:1;display:flex;flex-direction:column}
    .screen{display:none;animation:fadeIn .35s ease-out}
    .screen.active{display:block}

    @keyframes fadeIn{
      from{opacity:0;transform:translateY(6px)}
      to{opacity:1;transform:translateY(0)}
    }
    .screen-animate{
      animation:slideInUp .45s var(--transition-smooth);
    }
    @keyframes slideInUp{
      from{opacity:0;transform:translateY(25px) scale(.97)}
      to{opacity:1;transform:translateY(0) scale(1)}
    }

    /* LOGIN */

    .login-wrapper{
      max-width:440px;margin:70px auto;
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:22px;padding:28px;
      box-shadow:var(--shadow-soft);
      border:1px solid rgba(148,163,184,.28);
    }
    .login-title{
      font-size:24px;font-weight:800;margin-bottom:6px;text-align:center;
      background:linear-gradient(90deg,#22c55e,#0ea5e9);
      -webkit-background-clip:text;
      -webkit-text-fill-color:transparent;
    }
    .login-sub{
      font-size:14px;color:var(--text-muted);text-align:center;margin-bottom:20px;
    }
    .form-field{margin-bottom:14px}
    .form-label{font-size:13px;margin-bottom:4px;color:#9ca3af;display:flex;gap:6px;align-items:center}
    .text-input{
      width:100%;padding:11px 14px;border-radius:12px;
      border:1px solid #111827;
      background:rgba(15,23,42,.9);
      color:var(--text-main);font-size:14px;outline:none;
      transition:all var(--transition-fast);
    }
    .text-input:focus{
      border-color:#22c55e;
      box-shadow:0 0 0 1px rgba(34,197,94,.6);
    }
    .btn{
      border-radius:999px;border:none;
      padding:11px 20px;font-size:15px;font-weight:600;
      cursor:pointer;display:inline-flex;align-items:center;justify-content:center;
      gap:8px;transition:all var(--transition-fast);
    }
    .btn-full{width:100%}
    .btn-green{
      background:linear-gradient(90deg,#22c55e,#16a34a);
      color:#ecfdf5;
      box-shadow:0 8px 22px rgba(22,163,74,.7);
    }
    .btn-green:hover{transform:translateY(-1px) scale(1.01);}
    .btn-red{
      background:linear-gradient(90deg,#ef4444,#b91c1c);
      color:#fee2e2;
    }
    .btn-red:hover{transform:translateY(-1px) scale(1.01);}
    .btn-blue{
      background:linear-gradient(90deg,#0ea5e9,#2563eb);
      color:#e0f2fe;
    }
    .btn-blue:hover{transform:translateY(-1px) scale(1.01);}
    .btn-small{padding:7px 12px;font-size:12px}

    .sync-status{
      display:flex;align-items:center;gap:6px;font-size:12px;
      padding:7px 10px;border-radius:999px;margin-top:8px;
    }
    .sync-online{background:rgba(34,197,94,.12);color:#4ade80}
    .sync-offline{background:rgba(249,115,22,.12);color:#fdba74}

    /* HOME */

    .greeting{font-size:26px;font-weight:800;margin-bottom:6px;
      background:linear-gradient(90deg,#e5e7eb,#60a5fa);
      -webkit-background-clip:text;-webkit-text-fill-color:transparent;
    }
    .sub-greeting{font-size:15px;color:var(--text-muted);margin-bottom:22px}
    .tiles-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:18px}
    .tile{
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);
      padding:18px;box-shadow:var(--shadow-soft);
      border:1px solid rgba(148,163,184,.35);
      cursor:pointer;transition:all var(--transition-smooth);
      min-height:120px;display:flex;flex-direction:column;justify-content:space-between;
    }
    .tile:hover{
      transform:translateY(-4px) scale(1.01);
      box-shadow:var(--shadow-hover);
      border-color:#38bdf8;
    }
    .tile-header{display:flex;justify-content:space-between;gap:10px;margin-bottom:10px}
    .tile-icon{
      width:42px;height:42px;border-radius:14px;
      display:flex;align-items:center;justify-content:center;font-size:18px;color:#f9fafb;
      background:radial-gradient(circle at 20% 0,#22c55e 0,#22c55e 40%,#22d3ee 100%);
    }
    .tile-title{font-size:17px;font-weight:700;margin-bottom:6px}
    .tile-desc{font-size:13px;color:var(--text-muted);line-height:1.4}

    /* SECTIONS */

    .section-header{
      display:flex;align-items:center;justify-content:space-between;
      margin-bottom:18px;margin-top:6px;
    }
    .section-title{
      font-size:22px;font-weight:800;
      background:linear-gradient(90deg,#e5e7eb,#60a5fa);
      -webkit-background-clip:text;-webkit-text-fill-color:transparent;
    }
    .back-link{
      font-size:13px;color:#bbf7d0;
      cursor:pointer;display:inline-flex;align-items:center;gap:6px;
      padding:6px 12px;border-radius:999px;
      background:#064e3b;
      transition:all var(--transition-fast);
    }
    .back-link:hover{background:#059669;transform:translateY(-1px)}

    .search-bar{margin-bottom:16px;position:relative}
    .search-input{
      width:100%;padding:12px 40px 12px 14px;
      border-radius:999px;border:1px solid #111827;
      background:rgba(15,23,42,.95);
      color:var(--text-main);font-size:14px;
      outline:none;transition:all var(--transition-fast);
    }
    .search-input:focus{
      border-color:#38bdf8;
      box-shadow:0 0 0 1px rgba(56,189,248,.6);
    }
    .search-icon{
      position:absolute;top:50%;right:14px;
      transform:translateY(-50%);color:#6b7280;font-size:14px;
    }

    .teacher-list{display:flex;flex-direction:column;gap:10px}
    .teacher-card{
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);padding:14px 16px;
      border:1px solid #111827;
      display:flex;justify-content:space-between;align-items:center;
      cursor:pointer;transition:all var(--transition-smooth);
    }
    .teacher-card:hover{
      transform:translateY(-2px);
      box-shadow:var(--shadow-soft);
      border-color:#22c55e;
    }
    .teacher-main{display:flex;flex-direction:column;gap:3px}
    .teacher-name{font-size:15px;font-weight:600}
    .teacher-subject{font-size:13px;color:var(--text-muted)}
    .teacher-meta{display:flex;flex-direction:column;align-items:flex-end;gap:4px;font-size:12px}
    .feedback-counts{display:flex;gap:6px;align-items:center;font-size:11px}
    .count-pill{
      padding:3px 9px;border-radius:999px;font-weight:600;
      display:inline-flex;align-items:center;gap:4px;
    }
    .count-pill.pos{background:rgba(34,197,94,.16);color:#4ade80}
    .count-pill.neg{background:rgba(239,68,68,.16);color:#fecaca}

    .stat-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:14px;margin:16px 0 18px}
    .stat-card{
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);padding:18px;text-align:center;
      border:1px solid #111827;box-shadow:var(--shadow-soft);
    }
    .stat-label{font-size:13px;color:var(--text-muted);display:flex;justify-content:center;gap:6px;margin-bottom:6px}
    .stat-value{font-size:28px;font-weight:800}
    .stat-compliments .stat-value{color:#4ade80}
    .stat-remarks .stat-value{color:#fca5a5}
    .stat-behavior .stat-value{color:#60a5fa}

    .button-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:12px;margin-bottom:14px}

    .feedback-list{display:flex;flex-direction:column;gap:10px;margin-top:10px}
    .feedback-empty{
      font-size:14px;color:var(--text-muted);
      margin-top:18px;text-align:center;padding:26px;
      background:rgba(15,23,42,.9);border-radius:var(--radius-card);
      border:1px dashed #1f2937;
    }
    .feedback-item{
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);padding:14px;
      border:1px solid #111827;font-size:13px;
      display:flex;flex-direction:column;gap:6px;
    }
    .feedback-meta-line{display:flex;justify-content:space-between;gap:8px;font-size:11px;color:#9ca3af}
    .feedback-tags{display:flex;flex-wrap:wrap;gap:6px}
    .tag-pill{
      padding:3px 9px;border-radius:999px;font-size:11px;
      background:rgba(15,23,42,1);color:#e5e7eb;
      border:1px solid #1f2937;
    }

    .feedback-header{
      margin-bottom:16px;padding:18px;
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);
      box-shadow:var(--shadow-soft);
      border:1px solid #111827;
    }
    .feedback-header-title{font-size:18px;font-weight:700;margin-bottom:4px}
    .feedback-header-sub{font-size:13px;color:var(--text-muted)}

    .quick-tags{display:flex;flex-wrap:wrap;gap:8px;margin:12px 0 14px}
    .quick-tag{
      font-size:13px;padding:7px 14px;border-radius:999px;
      border:1px solid #1f2937;
      background:rgba(15,23,42,.95);
      cursor:pointer;transition:all var(--transition-fast);
      color:#e5e7eb;   /* ← שלא יעלם הטקסט */
    }
    .quick-tag:hover{
      transform:translateY(-2px);
      box-shadow:0 6px 14px rgba(15,23,42,.8);
      background:#020617;
    }
    .quick-tag.selected-positive{
      background:rgba(34,197,94,.16);
      border-color:#22c55e;
      color:#bbf7d0;
      font-weight:600;
    }
    .quick-tag.selected-negative{
      background:rgba(248,113,113,.18);
      border-color:#f97373;
      color:#fecaca;
      font-weight:600;
    }

    .textarea{
      width:100%;min-height:120px;resize:vertical;
      border-radius:18px;border:1px solid #111827;
      padding:14px 16px;font-size:14px;
      background:rgba(15,23,42,.96);color:var(--text-main);
      outline:none;margin-bottom:14px;
    }
    .textarea:focus{
      border-color:#22c55e;
      box-shadow:0 0 0 1px rgba(34,197,94,.6);
    }

    /* REPORTS & LEADERBOARD */

    .reports-section{display:flex;flex-direction:column;gap:14px}
    .report-summary-card{
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);padding:18px;
      box-shadow:var(--shadow-soft);border:1px solid #111827;
    }
    .report-row{
      display:flex;justify-content:space-between;
      font-size:13px;margin-top:6px;color:var(--text-muted);
      padding:4px 0;border-bottom:1px solid #020617;
    }
    .report-highlight{
      margin-top:10px;font-size:13px;color:#e5e7eb;
      padding:10px 12px;border-radius:10px;
      background:rgba(15,23,42,.95);border:1px solid #111827;
    }

    .leaderboard-table{width:100%;border-collapse:collapse;margin-top:8px;font-size:13px}
    .leaderboard-table th,
    .leaderboard-table td{
      padding:8px 10px;text-align:right;border-bottom:1px solid #020617;
    }
    .leaderboard-table th{
      font-size:12px;color:#9ca3af;background:#020617;position:sticky;top:0;
    }
    .leaderboard-bar-wrapper{
      width:100%;background:#020617;border-radius:999px;overflow:hidden;height:7px;
    }
    .leaderboard-bar{height:100%;background:linear-gradient(90deg,#22c55e,#38bdf8);border-radius:999px}

    .chip{
      display:inline-flex;align-items:center;gap:6px;
      padding:6px 12px;border-radius:999px;
      background:#020617;border:1px solid #111827;
      font-size:12px;color:#e5e7eb;
    }

    /* ADMIN & BUTTONS */

    .admin-grid{display:grid;grid-template-columns:minmax(0,1.2fr) minmax(0,1fr);gap:16px;margin-top:16px}
    .admin-panel{
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:var(--radius-card);padding:18px;
      box-shadow:var(--shadow-soft);border:1px solid #111827;
    }
    .admin-panel-title{font-size:16px;font-weight:700;margin-bottom:10px;display:flex;align-items:center;gap:6px}
    .admin-form-row{display:flex;gap:10px;margin-bottom:12px}
    .admin-note{font-size:12px;color:var(--text-muted);margin-top:6px;display:flex;gap:6px;align-items:center}

    .feedback-actions{display:flex;gap:8px;margin-top:6px}
    .delete-feedback-btn{
      background:rgba(239,68,68,.1);color:#fecaca;
      border:1px solid rgba(248,113,113,.5);
      border-radius:999px;padding:3px 9px;font-size:11px;
      cursor:pointer;transition:all .18s;
    }
    .delete-feedback-btn:hover{background:rgba(239,68,68,.22)}

    /* NOTIF PANEL */

    .notif-panel{
      position:fixed;top:70px;right:20px;
      width:min(360px,90vw);max-height:70vh;
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      border-radius:18px;box-shadow:0 20px 40px rgba(0,0,0,.8);
      border:1px solid rgba(148,163,184,.35);
      padding:14px;display:flex;flex-direction:column;
      opacity:0;transform:translateY(-10px);pointer-events:none;
      transition:opacity .28s ease-out,transform .28s ease-out;
      z-index:1000;
    }
    .notif-panel.open{opacity:1;transform:translateY(0);pointer-events:auto}
    .notif-header{font-size:15px;font-weight:700;margin-bottom:4px;display:flex;justify-content:space-between}
    .notif-sub{font-size:12px;color:var(--text-muted);margin-bottom:8px}
    .notif-list{overflow-y:auto;padding-right:2px;max-height:52vh}
    .notif-item{
      background:#020617;border-radius:12px;padding:10px;
      font-size:13px;margin-bottom:6px;
      border:1px solid #111827;
      display:flex;gap:8px;
    }
    .notif-icon{
      width:26px;height:26px;border-radius:999px;
      display:flex;align-items:center;justify-content:center;font-size:13px;
    }
    .notif-icon.good{background:rgba(34,197,94,.16);color:#4ade80}
    .notif-icon.bad{background:rgba(248,113,113,.16);color:#fca5a5}
    .notif-icon.info{background:rgba(59,130,246,.16);color:#bfdbfe}
    .notif-title{font-weight:600;margin-bottom:2px}
    .notif-text{font-size:12px;color:#d1d5db}

    /* TOAST & FX */

    @keyframes toastIn{
      from{opacity:0;transform:translate(-50%,10px) scale(.96)}
      to{opacity:1;transform:translate(-50%,0) scale(1)}
    }
    @keyframes toastOut{
      from{opacity:1;transform:translate(-50%,0) scale(1)}
      to{opacity:0;transform:translate(-50%,-8px) scale(.97)}
    }
    .feedback-toast{
      position:fixed;top:92px;left:50%;transform:translateX(-50%);
      background:radial-gradient(circle at top,#020617 0,#02081a 55%,#020617 100%);
      color:#f9fafb;padding:14px 18px;border-radius:16px;
      box-shadow:0 14px 40px rgba(0,0,0,.85);
      border:1px solid #111827;
      display:flex;align-items:center;gap:10px;
      z-index:1200;max-width:420px;width:92%;
      animation:toastIn .28s ease-out;
    }
    .feedback-toast.fade-out{animation:toastOut .3s ease-in forwards}
    .feedback-toast.compliment{border-inline-start:4px solid #22c55e}
    .feedback-toast.remark{border-inline-start:4px solid #ef4444}
    .toast-icon{
      width:40px;height:40px;border-radius:999px;
      display:flex;align-items:center;justify-content:center;font-size:18px;
    }
    .toast-icon.compliment{background:rgba(34,197,94,.16);color:#4ade80}
    .toast-icon.remark{background:rgba(248,113,113,.16);color:#fecaca}
    .toast-content{flex:1}
    .toast-title{font-weight:700;margin-bottom:3px;font-size:14px}
    .toast-message{font-size:13px;color:#e5e7eb}

    .pulse-animation{animation:pulse .5s ease-in-out}
    @keyframes pulse{
      0%{transform:scale(1)}
      50%{transform:scale(1.04)}
      100%{transform:scale(1)}
    }
    .shake-animation{animation:shake .4s ease-in-out}
    @keyframes shake{
      0%,100%{transform:translateX(0)}
      25%{transform:translateX(-4px)}
      75%{transform:translateX(4px)}
    }

    .floating-particles{
      position:fixed;top:0;left:0;width:100%;height:100%;
      pointer-events:none;z-index:900;
    }
    .particle{
      position:absolute;width:8px;height:8px;border-radius:50%;pointer-events:none;
    }
    .particle.heart{background:#f97373;animation:floatUp 1.4s ease-out forwards}
    .particle.star{background:#fbbf24;animation:floatUp 1.8s ease-out forwards}
    .particle.sparkle{background:#38bdf8;animation:floatUp 1.6s ease-out forwards}
    @keyframes floatUp{
      0%{transform:translateY(0) scale(1);opacity:1}
      100%{transform:translateY(-90px) scale(.4);opacity:0}
    }
  </style>
</head>
<body>
<div class="app">
  <!-- Particles -->
  <div id="floating-particles" class="floating-particles"></div>

  <!-- Notifications panel -->
  <div id="notif-panel" class="notif-panel" aria-hidden="true">
    <div class="notif-header">
      <span>התראות חכמות</span>
      <span style="font-size:12px;color:#9ca3af;">מבוסס על המשובים שלך</span>
    </div>
    <div class="notif-sub">אין ספאם – רק תובנות קצרות שעוזרות לך.</div>
    <div id="notif-list" class="notif-list"></div>
  </div>

  <!-- HEADER -->
  <header class="app-header">
    <div class="app-header-left">
      <div class="logo"><i class="fas fa-chalkboard-teacher"></i></div>
      <div>
        <div class="app-title">SchoolRank</div>
        <div class="brand-small">מערכת משוב למורים</div>
      </div>
    </div>
    <div class="app-header-right">
      <div class="icon-button" title="התראות" id="notif-button">
        <i class="fas fa-bell"></i>
        <span id="notif-badge" class="notif-badge" style="display:none;"></span>
      </div>
      <div class="user-chip" id="user-chip">
        <i class="fas fa-user"></i>
        <span>לא מחובר</span>
      </div>
      <div class="icon-button" title="עזרה" id="help-button">
        <i class="fas fa-question"></i>
      </div>
    </div>
  </header>

  <!-- MAIN -->
  <main class="app-main">

    <!-- LOGIN -->
    <section id="screen-login" class="screen active">
      <div class="login-wrapper">
        <div class="login-title">התחברות למערכת</div>
        <div class="login-sub">
          אתה יכול לכתוב כל שם משתמש וכל סיסמה – המערכת תשמור אותם במחשב שלך.<br>
          לדוגמה: <b>תלמיד</b> או כל שם אחר שבא לך.<br>
          <b>בפעם הבאה תיכנס אוטומטית בעזרת קוקיז.</b>
        </div>

        <div class="form-field">
          <div class="form-label"><i class="fas fa-user"></i><span>שם משתמש</span></div>
          <input type="text" id="login-username" class="text-input" placeholder="לדוגמה: תלמיד">
        </div>
        <div class="form-field">
          <div class="form-label"><i class="fas fa-lock"></i><span>סיסמה</span></div>
          <input type="password" id="login-password" class="text-input" placeholder="אפשר כל דבר">
        </div>
        <button class="btn btn-full btn-green" id="login-button">
          <i class="fas fa-sign-in-alt"></i><span>התחברות</span>
        </button>

        <div style="margin-top:14px;padding:10px 12px;background:rgba(37,99,235,.12);border-radius:12px;border:1px solid rgba(59,130,246,.45);">
          <div style="font-size:12px;color:#bfdbfe;display:flex;align-items:center;gap:6px;">
            <i class="fas fa-cloud"></i>
            <span>המערכת מחוברת ל-Supabase – המשובים נשמרים ונראים לכולם.</span>
          </div>
          <div id="sync-status" class="sync-status sync-online">
            <i class="fas fa-database"></i><span>מחובר - נתונים משותפים</span>
          </div>
        </div>
      </div>
    </section>

    <!-- HOME -->
    <section id="screen-home" class="screen">
      <div class="greeting" id="home-greeting">מה אתה רוצה לעשות היום?</div>
      <div class="sub-greeting" id="home-sub">מערכת משוב. לא מושלמת, אבל בצד שלך.</div>

      <div class="tiles-grid" id="home-tiles">
        <div class="tile" data-action="view-teachers">
          <div class="tile-header">
            <div>
              <div class="tile-title">צפייה במורים</div>
              <div class="tile-desc">לעבור על כל המורים, לראות מי זוכה בנקודות.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-users"></i></div>
          </div>
        </div>

        <div class="tile" data-action="quick-compliment">
          <div class="tile-header">
            <div>
              <div class="tile-title">הוספת מחמאה</div>
              <div class="tile-desc">הסביר ברור, היה נחמד, היה אנושי – מגיע לו.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-heart"></i></div>
          </div>
        </div>

        <div class="tile" data-action="quick-remark">
          <div class="tile-header">
            <div>
              <div class="tile-title">הוספת הערה</div>
              <div class="tile-desc">לא ברור, לא הוגן, או פשוט לא. כותבים את זה.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-exclamation-triangle"></i></div>
          </div>
        </div>

        <div class="tile" data-action="view-reports">
          <div class="tile-header">
            <div>
              <div class="tile-title">הדוחות שלי</div>
              <div class="tile-desc">כל מה שכבר כתבת – טוב ורע – במקום אחד.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-chart-bar"></i></div>
          </div>
        </div>

        <div class="tile" data-action="leaderboard">
          <div class="tile-header">
            <div>
              <div class="tile-title">לוח מדרגים</div>
              <div class="tile-desc">מי הכי פעיל במתן משובים.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-trophy"></i></div>
          </div>
        </div>

        <div class="tile" data-action="admin" id="admin-tile" style="display:none;">
          <div class="tile-header">
            <div>
              <div class="tile-title">ניהול מורים (אדמין)</div>
              <div class="tile-desc">הוספת מורים, מחיקה וניהול רשימה.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-cog"></i></div>
          </div>
        </div>
      </div>
    </section>

    <!-- TEACHER LIST -->
    <section id="screen-teachers" class="screen">
      <div class="section-header">
        <div class="section-title">רשימת מורים</div>
        <div class="back-link" data-back-to="home"><i class="fas fa-arrow-right"></i><span>חזרה</span></div>
      </div>
      <div class="search-bar">
        <input id="teacher-search" class="search-input" type="text" placeholder="חיפוש לפי שם או מקצוע...">
        <div class="search-icon"><i class="fas fa-search"></i></div>
      </div>
      <div class="teacher-list" id="teacher-list"></div>
    </section>

    <!-- TEACHER PROFILE -->
    <section id="screen-teacher-profile" class="screen">
      <div class="section-header">
        <div class="section-title" id="teacher-profile-name">שם מורה</div>
        <div class="back-link" data-back-to="teachers"><i class="fas fa-arrow-right"></i><span>חזרה</span></div>
      </div>

      <div class="stat-row">
        <div class="stat-card stat-compliments">
          <div class="stat-label"><i class="fas fa-heart"></i><span>סה״כ מחמאות</span></div>
          <div class="stat-value" id="stat-compliments">0</div>
        </div>
        <div class="stat-card stat-remarks">
          <div class="stat-label"><i class="fas fa-exclamation-triangle"></i><span>סה״כ הערות</span></div>
          <div class="stat-value" id="stat-remarks">0</div>
        </div>
        <div class="stat-card stat-behavior">
          <div class="stat-label"><i class="fas fa-star"></i><span>ניקוד התנהגותי</span></div>
          <div class="stat-value" id="stat-behavior">0</div>
        </div>
      </div>

      <div class="button-row">
        <button class="btn btn-green" id="btn-profile-compliment"><i class="fas fa-heart"></i><span>הוסף מחמאה</span></button>
        <button class="btn btn-red" id="btn-profile-remark"><i class="fas fa-exclamation-triangle"></i><span>הוסף הערה</span></button>
      </div>

      <h3 style="font-size:18px;margin:16px 0 10px;"><i class="fas fa-history"></i> <span>משובים אחרונים</span></h3>
      <div id="teacher-feedback-list" class="feedback-list"></div>
      <div id="teacher-feedback-empty" class="feedback-empty">
        <i class="fas fa-inbox"></i><div>אין עדיין משוב.</div>
      </div>
    </section>

    <!-- FEEDBACK FORM -->
    <section id="screen-feedback" class="screen">
      <div class="section-header">
        <div class="section-title">הוספת משוב</div>
        <div class="back-link" id="feedback-back"><i class="fas fa-arrow-right"></i><span>חזרה</span></div>
      </div>

      <div class="feedback-header">
        <div class="feedback-header-title" id="feedback-title"></div>
        <div class="feedback-header-sub" id="feedback-subtitle"></div>
      </div>

      <div class="quick-tags" id="quick-tags"></div>
      <textarea id="feedback-text" class="textarea" placeholder="תכתוב מה שחשוב לך."></textarea>

      <button class="btn btn-full btn-green" id="feedback-submit-button">
        <i class="fas fa-paper-plane"></i><span>שליחת משוב</span>
      </button>
    </section>

    <!-- REPORTS -->
    <section id="screen-reports" class="screen">
      <div class="section-header">
        <div class="section-title">הדוחות שלי</div>
        <div class="back-link" data-back-to="home"><i class="fas fa-arrow-right"></i><span>חזרה</span></div>
      </div>

      <div class="reports-section">
        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
            <i class="fas fa-chart-pie"></i><strong>סיכום כללי</strong>
          </div>
          <div class="report-row"><span>סה״כ מחמאות</span><span id="reports-total-compliments">0</span></div>
          <div class="report-row"><span>סה״כ הערות</span><span id="reports-total-remarks">0</span></div>
        </div>

        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
            <i class="fas fa-calendar-week"></i><strong>סיכום שבועי</strong>
          </div>
          <div class="report-row"><span>מחמאות בשבוע האחרון</span><span id="reports-week-compliments">0</span></div>
          <div class="report-row"><span>הערות בשבוע האחרון</span><span id="reports-week-remarks">0</span></div>
          <div class="report-highlight" id="reports-week-summary-text">עדיין אין נתונים לשבוע הזה.</div>
        </div>

        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
            <i class="fas fa-clock"></i><strong>פעילות אחרונה</strong>
          </div>
          <div id="reports-latest-list" class="feedback-list"></div>
          <div id="reports-empty" class="feedback-empty">
            <i class="fas fa-inbox"></i><div>עדיין אין משובים.</div>
          </div>
        </div>
      </div>
    </section>

    <!-- LEADERBOARD -->
    <section id="screen-leaderboard" class="screen">
      <div class="section-header">
        <div class="section-title">לוח מדרגים</div>
        <div class="back-link" data-back-to="home"><i class="fas fa-arrow-right"></i><span>חזרה</span></div>
      </div>

      <div class="chip"><i class="fas fa-trophy"></i><span>מי נותן הכי הרבה משובים</span></div>

      <div class="reports-section" style="margin-top:16px;">
        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px;">
            <i class="fas fa-users"></i><strong>טבלת תלמידים</strong>
          </div>
          <table class="leaderboard-table">
            <thead>
              <tr>
                <th>#</th><th>שם משתמש</th><th>מחמאות</th><th>הערות</th><th>סה״כ</th><th>פעילות</th>
              </tr>
            </thead>
            <tbody id="leaderboard-body"></tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- ADMIN -->
    <section id="screen-admin" class="screen">
      <div class="section-header">
        <div class="section-title">ניהול מורים</div>
        <div class="back-link" data-back-to="home"><i class="fas fa-arrow-right"></i><span>חזרה</span></div>
      </div>

      <div class="chip"><i class="fas fa-shield-alt"></i><span>מצב אדמין – רק אתה רואה את זה</span></div>

      <div class="admin-grid">
        <div class="admin-panel">
          <div class="admin-panel-title"><i class="fas fa-user-plus"></i><span>הוספת מורה חדש</span></div>
          <div class="admin-form-row">
            <input type="text" id="admin-new-name" class="text-input" placeholder="שם המורה">
            <input type="text" id="admin-new-subject" class="text-input" placeholder="מקצוע">
          </div>
          <button class="btn btn-green btn-full" id="admin-add-teacher"><i class="fas fa-plus"></i><span>הוסף מורה</span></button>
          <div class="admin-note"><i class="fas fa-info-circle"></i><span>הנתונים נשמרים ב-Supabase ונראים לכל המשתמשים.</span></div>
        </div>

        <div class="admin-panel">
          <div class="admin-panel-title"><i class="fas fa-list"></i><span>רשימת מורים (אדמין)</span></div>
          <div id="admin-teacher-list" class="teacher-list"></div>
        </div>
      </div>

      <div class="admin-panel" style="margin-top:16px;">
        <div class="admin-panel-title"><i class="fas fa-comments"></i><span>ניהול משובים</span></div>
        <div class="search-bar">
          <input id="admin-feedback-search" class="search-input" type="text" placeholder="חיפוש משובים...">
          <div class="search-icon"><i class="fas fa-search"></i></div>
        </div>
        <div id="admin-feedback-list" class="feedback-list"></div>
        <div id="admin-feedback-empty" class="feedback-empty">
          <i class="fas fa-inbox"></i><div>אין משובים.</div>
        </div>
      </div>
    </section>

  </main>
</div>

<script>
  // ---------- Supabase ----------
  const SUPABASE_URL = 'https://xidfthnboggokcsloglt.supabase.co';
  const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhpZGZ0aG5ib2dnb2tjc2xvZ2x0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjQzMTIwNzUsImV4cCI6MjA3OTg4ODA3NX0.fEPS2FJYlcZ4DOv7I0RBEcwZBfT0MdRGslk9cp_2GwU';
  const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

  // ---------- Global Data ----------
  let teachers = [];
  let feedbackEntries = [];
  let studentStats = {};

  const quickTagSets = {
    compliment: ["מסביר ברור", "יחס טוב", "שומר על סדר", "נותן משוב מועיל"],
    remark: ["הסבר לא ברור", "ציון לא הוגן", "דיבור לא מכבד", "מאחר לשיעור"]
  };

  const appState = {
    currentScreen: 'login',
    currentUser: null,
    selectedTeacherId: null,
    feedbackType: null
  };

  // ---------- Sounds & FX - גיימינג ----------
  const sounds = {
    send:   new Audio('https://assets.mixkit.co/sfx/download/mixkit-video-game-win-2016.wav'),
    error:  new Audio('https://assets.mixkit.co/sfx/download/mixkit-retro-arcade-lose-2027.wav'),
    click:  new Audio('https://assets.mixkit.co/sfx/download/mixkit-arcade-mechanical-bling-210.wav'),
    open:   new Audio('https://assets.mixkit.co/sfx/download/mixkit-positive-interface-click-1112.wav'),
    notif:  new Audio('https://assets.mixkit.co/sfx/download/mixkit-software-interface-back-2575.wav')
  };
  Object.values(sounds).forEach(s => { s.volume = 0.4; });

  function playSound(name){
    const sound = sounds[name];
    if(!sound) return;
    sound.currentTime = 0;
    sound.play().catch(()=>{});
  }

  // ---------- Cookies ----------
  function setCookie(name,value,days){
    const expires = new Date();
    expires.setTime(expires.getTime()+days*24*60*60*1000);
    document.cookie = name+'='+value+';expires='+expires.toUTCString()+';path=/';
  }
  function getCookie(name){
    const nameEQ=name+'=';
    const ca=document.cookie.split(';');
    for(let c of ca){
      while(c.charAt(0)===' ') c=c.substring(1);
      if(c.indexOf(nameEQ)===0) return c.substring(nameEQ.length);
    }
    return null;
  }
  function deleteCookie(name){
    document.cookie=name+'=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;';
  }

  // ---------- FX ----------
  function showFeedbackAnimation(type, teacherName){
    const toast=document.createElement('div');
    toast.className='feedback-toast '+type;

    const iconClass = type==='compliment' ? 'fas fa-heart' : 'fas fa-exclamation-triangle';
    const title     = type==='compliment' ? 'מחמאה נשלחה!' : 'הערה נשלחה';
    const msg       = type==='compliment'
      ? `המחמאה למורה ${teacherName} נשמרה בהצלחה.`
      : `ההערה למורה ${teacherName} נרשמה במערכת.`;

    toast.innerHTML = `
      <div class="toast-icon ${type}">
        <i class="${iconClass}"></i>
      </div>
      <div class="toast-content">
        <div class="toast-title">${title}</div>
        <div class="toast-message">${msg}</div>
      </div>
    `;
    document.body.appendChild(toast);

    const submitBtn=document.getElementById('feedback-submit-button');
    if(submitBtn){
      submitBtn.classList.add('pulse-animation');
      setTimeout(()=>submitBtn.classList.remove('pulse-animation'),500);
    }

    if(type==='compliment') createFloatingParticles();

    setTimeout(()=>{
      toast.classList.add('fade-out');
      setTimeout(()=>toast.remove(),280);
    },2600);
  }

  function createFloatingParticles(){
    const container=document.getElementById('floating-particles');
    const types=['heart','star','sparkle'];
    for(let i=0;i<15;i++){
      setTimeout(()=>{
        const p=document.createElement('div');
        const t=types[Math.floor(Math.random()*types.length)];
        p.className='particle '+t;
        p.style.left=Math.random()*100+'%';
        p.style.top=80+Math.random()*15+'%';
        p.style.animationDelay=(Math.random()*0.4)+'s';
        container.appendChild(p);
        setTimeout(()=>p.remove(),1800);
      },i*90);
    }
  }

  function showErrorAnimation(){
    const btn=document.getElementById('feedback-submit-button');
    if(!btn) return;
    btn.classList.add('shake-animation');
    playSound('error');
    setTimeout(()=>btn.classList.remove('shake-animation'),400);
  }

  // ---------- Data Load ----------
  async function loadData(){
    try{
      const { data: teachersData, error: teachersError } =
        await supabase.from('teachers').select('*').order('id');
      if(teachersError) throw teachersError;
      teachers = teachersData || [];

      const { data: feedbackData, error: feedbackError } =
        await supabase.from('feedback').select('*').order('created_at',{ascending:false});
      if(feedbackError) throw feedbackError;
      feedbackEntries = feedbackData || [];

      const { data: statsData, error: statsError } =
        await supabase.from('student_stats').select('*');
      if(statsError) throw statsError;

      studentStats = {};
      (statsData||[]).forEach(st=>{
        studentStats[st.user_name]={compliments:st.compliments,remarks:st.remarks};
      });

      updateSyncStatus(true);
      updateNotifications();
    }catch(err){
      console.error('שגיאה בטעינת נתונים',err);
      teachers = [
        { id:1,name:'אורן',subject:'אנגלית'},
        { id:2,name:'אורית',subject:'מתמטיקה'},
        { id:3,name:'רעות',subject:'לשון'},
        { id:4,name:'אבי',subject:'השכלה כללית'},
        { id:5,name:'נטע',subject:'היסטוריה'}
      ];
      feedbackEntries = [];
      studentStats = {};
      updateSyncStatus(false);
    }
  }

  async function saveFeedback(feedback){
    try{
      const { data, error } = await supabase
        .from('feedback')
        .insert([{
          teacher_id: feedback.teacherId,
          type:       feedback.type,
          tags:       feedback.tags,
          text:       feedback.text,
          user_name:  feedback.user
        }])
        .select();
      if(error) throw error;

      const inserted = data[0];

      await updateStudentStats(feedback.user, feedback.type);

      // עדכון מקומי מיידי – שלא נצטרך רענון
      feedbackEntries.unshift(inserted);
      if(!studentStats[feedback.user]){
        studentStats[feedback.user]={compliments:0,remarks:0};
      }
      if(feedback.type==='compliment') studentStats[feedback.user].compliments++;
      else studentStats[feedback.user].remarks++;

      updateNotifications();
      return inserted;
    }catch(err){
      console.error('שגיאה בשמירת משוב',err);
      return null;
    }
  }

  async function deleteFeedback(feedbackId){
    try{
      const { error } = await supabase.from('feedback').delete().eq('id',feedbackId);
      if(error) throw error;
      feedbackEntries = feedbackEntries.filter(f=>f.id!==feedbackId);
      updateNotifications();
      return true;
    }catch(err){
      console.error('שגיאה במחיקת משוב',err);
      return false;
    }
  }

  async function updateStudentStats(userName,type){
    try{
      const { data: existing, error: checkError } =
        await supabase.from('student_stats').select('*').eq('user_name',userName).single();

      if(checkError && checkError.code!=='PGRST116') throw checkError;

      if(existing){
        const updateData = type==='compliment'
          ? { compliments: existing.compliments+1 }
          : { remarks: existing.remarks+1 };
        const { error:updateError } =
          await supabase.from('student_stats').update(updateData).eq('user_name',userName);
        if(updateError) throw updateError;
      }else{
        const newStat = {
          user_name:userName,
          compliments:type==='compliment'?1:0,
          remarks:type==='remark'?1:0
        };
        const { error:insertError } =
          await supabase.from('student_stats').insert([newStat]);
        if(insertError) throw insertError;
      }
      return true;
    }catch(err){
      console.error('שגיאה בעדכון student_stats',err);
      return false;
    }
  }

  async function addTeacher(name,subject){
    try{
      const { data, error } = await supabase
        .from('teachers').insert([{name,subject}]).select();
      if(error) throw error;
      const t=data[0];
      teachers.push(t);
      return t;
    }catch(err){
      console.error('שגיאה בהוספת מורה',err);
      return null;
    }
  }

  async function deleteTeacher(teacherId){
    try{
      const { error: fErr } = await supabase.from('feedback').delete().eq('teacher_id',teacherId);
      if(fErr) throw fErr;
      const { error: tErr } = await supabase.from('teachers').delete().eq('id',teacherId);
      if(tErr) throw tErr;
      teachers = teachers.filter(t=>t.id!==teacherId);
      feedbackEntries = feedbackEntries.filter(f=>f.teacher_id!==teacherId);
      updateNotifications();
      return true;
    }catch(err){
      console.error('שגיאה במחיקת מורה',err);
      return false;
    }
  }

  // ---------- Realtime ----------
  function setupRealtimeUpdates(){
    supabase
      .channel('feedback-changes')
      .on('postgres_changes',
        {event:'*',schema:'public',table:'feedback'},
        payload=>{ console.log('feedback realtime',payload); loadData(); }
      ).subscribe();

    supabase
      .channel('teachers-changes')
      .on('postgres_changes',
        {event:'*',schema:'public',table:'teachers'},
        payload=>{ console.log('teachers realtime',payload); loadData(); }
      ).subscribe();
  }

  function updateSyncStatus(isOnline){
    const el=document.getElementById('sync-status');
    if(!el) return;
    if(isOnline){
      el.className='sync-status sync-online';
      el.innerHTML='<i class="fas fa-database"></i><span>מחובר - נתונים משותפים</span>';
    }else{
      el.className='sync-status sync-offline';
      el.innerHTML='<i class="fas fa-cloud-slash"></i><span>לא מחובר - נתונים מקומיים בלבד</span>';
    }
  }

  // ---------- Helpers ----------
  function showScreen(name){
    document.querySelectorAll('.screen').forEach(s=>{
      s.classList.toggle('active', s.id==='screen-'+name);
      if(s.id==='screen-'+name){
        s.classList.add('screen-animate');
        setTimeout(()=>s.classList.remove('screen-animate'),350);
      }
    });
    appState.currentScreen=name;

    if(name==='teachers') renderTeacherList();
    if(name==='teacher-profile') renderTeacherProfile();
    if(name==='feedback') renderFeedbackScreen();
    if(name==='reports') renderReportsScreen();
    if(name==='leaderboard') renderLeaderboardScreen();
    if(name==='admin') renderAdminScreen();

    if(name==='teacher-profile' || name==='reports' || name==='leaderboard'){
      playSound('open');
    }
  }

  function getTeacherById(id){ return teachers.find(t=>t.id===id); }

  function getTeacherStats(id){
    const entries=feedbackEntries.filter(f=>f.teacher_id===id);
    const compliments = entries.filter(f=>f.type==='compliment').length;
    const remarks     = entries.filter(f=>f.type==='remark').length;
    const score       = compliments - remarks;
    return { compliments, remarks, score };
  }

  function getDisplayNameForUser(user){ return user?.username || 'תלמיד'; }
  function formatDateShort(d){ return new Date(d).toLocaleDateString('he-IL'); }

  // ---------- Login ----------
  function handleLogin(){
    const username=document.getElementById('login-username').value.trim();
    const password=document.getElementById('login-password').value.trim();
    if(!username){
      alert('צריך לכתוב שם משתמש.');
      return;
    }
    const user={username,role:'student'};
    if(username==='adir' && password==='1234'){ user.role='admin'; }

    appState.currentUser=user;
    setCookie('teacherFeedbackUser',JSON.stringify(user),30);

    document.getElementById('user-chip').innerHTML =
      `<i class="fas fa-user"></i><span>מחובר: ${user.role==='admin'?'אדמין':'תלמיד'}</span>`;
    document.getElementById('admin-tile').style.display =
      user.role==='admin' ? 'block' : 'none';

    playSound('click');
    showScreen('home');
    updateNotifications();
  }

  function handleLogout(){
    appState.currentUser=null;
    deleteCookie('teacherFeedbackUser');
    document.getElementById('user-chip').innerHTML='<i class="fas fa-user"></i><span>לא מחובר</span>';
    document.getElementById('admin-tile').style.display='none';
    showScreen('login');
  }

  function autoLoginUser(){
    const cookie=getCookie('teacherFeedbackUser');
    if(!cookie) return;
    try{
      const user=JSON.parse(cookie);
      appState.currentUser=user;
      document.getElementById('user-chip').innerHTML =
        `<i class="fas fa-user"></i><span>מחובר: ${user.role==='admin'?'אדמין':'תלמיד'}</span>`;
      document.getElementById('admin-tile').style.display =
        user.role==='admin' ? 'block' : 'none';
      showScreen('home');
      updateNotifications();
      console.log('Auto login from cookie');
    }catch(err){
      console.error('שגיאה ב-cookie',err);
      deleteCookie('teacherFeedbackUser');
    }
  }

  // ---------- Render ----------
  function renderTeacherList(){
    const container=document.getElementById('teacher-list');
    const search=document.getElementById('teacher-search').value.trim().toLowerCase();
    const filtered=teachers.filter(t=>{
      if(!search) return true;
      return t.name.toLowerCase().includes(search) ||
             t.subject.toLowerCase().includes(search);
    });
    container.innerHTML='';
    if(filtered.length===0){
      container.innerHTML='<div class="feedback-empty">לא נמצאו מורים.</div>';
      return;
    }
    filtered.forEach(t=>{
      const stats=getTeacherStats(t.id);
      const card=document.createElement('div');
      card.className='teacher-card';
      card.innerHTML=`
        <div class="teacher-main">
          <div class="teacher-name">${t.name}</div>
          <div class="teacher-subject">${t.subject}</div>
        </div>
        <div class="teacher-meta">
          <div class="feedback-counts">
            <div class="count-pill pos"><i class="fas fa-heart"></i>${stats.compliments}</div>
            <div class="count-pill neg"><i class="fas fa-exclamation-triangle"></i>${stats.remarks}</div>
          </div>
          <div style="font-size:12px;color:#9ca3af;">ניקוד: ${stats.score>0?'+':''}${stats.score}</div>
        </div>
      `;
      card.addEventListener('click',()=>{
        playSound('click');
        appState.selectedTeacherId=t.id;
        showScreen('teacher-profile');
      });
      container.appendChild(card);
    });
  }

  function renderTeacherProfile(){
    const teacher=getTeacherById(appState.selectedTeacherId);
    if(!teacher) return;

    document.getElementById('teacher-profile-name').textContent=teacher.name;

    const stats=getTeacherStats(teacher.id);
    document.getElementById('stat-compliments').textContent=stats.compliments;
    document.getElementById('stat-remarks').textContent=stats.remarks;
    document.getElementById('stat-behavior').textContent=stats.score>0?('+'+stats.score):stats.score;

    const entries=feedbackEntries
      .filter(f=>f.teacher_id===teacher.id)
      .sort((a,b)=>new Date(b.created_at)-new Date(a.created_at));

    const list=document.getElementById('teacher-feedback-list');
    const empty=document.getElementById('teacher-feedback-empty');
    list.innerHTML='';
    if(entries.length===0){
      empty.style.display='block';
      list.style.display='none';
      return;
    }
    empty.style.display='none';
    list.style.display='flex';

    entries.slice(0,10).forEach(e=>{
      const item=document.createElement('div');
      item.className='feedback-item';

      const deleteButton = appState.currentUser?.role==='admin'
        ? `<button class="delete-feedback-btn" data-feedback-id="${e.id}">
             <i class="fas fa-trash"></i> מחק
           </button>` : '';

      item.innerHTML=`
        <div>${e.text || '<i>אין טקסט</i>'}</div>
        <div class="feedback-tags">
          ${(e.tags||[]).map(tag=>`<span class="tag-pill">${tag}</span>`).join('')}
        </div>
        <div class="feedback-meta-line">
          <span>${e.type==='compliment'?'👍 מחמאה':'⚠️ הערה'} · ${e.user_name}</span>
          <span>${formatDateShort(e.created_at)}</span>
        </div>
        ${deleteButton ? `<div class="feedback-actions">${deleteButton}</div>`:''}
      `;

      if(appState.currentUser?.role==='admin'){
        const btn=item.querySelector('.delete-feedback-btn');
        btn.addEventListener('click',async ev=>{
          ev.stopPropagation();
          if(confirm('למחוק את המשוב?')){
            const ok=await deleteFeedback(e.id);
            if(ok){
              renderTeacherProfile();
              renderAdminFeedbackList();
            }
          }
        });
      }
      list.appendChild(item);
    });
  }

  function renderFeedbackScreen(){
    const teacher=getTeacherById(appState.selectedTeacherId);
    if(!teacher) return;
    const type=appState.feedbackType;
    const isCompliment=type==='compliment';

    document.getElementById('feedback-title').textContent =
      `${isCompliment?'הוספת מחמאה':'הוספת הערה'} למורה ${teacher.name}`;
    document.getElementById('feedback-subtitle').textContent =
      isCompliment ? 'תכתוב מה היה טוב.' : 'תכתוב מה מפריע לך (בלי קללות).';

    const quickTagsEl=document.getElementById('quick-tags');
    quickTagsEl.innerHTML='';
    const tags=quickTagSets[type];
    tags.forEach(tag=>{
      const btn=document.createElement('button');
      btn.type='button';
      btn.className='quick-tag';
      btn.textContent=tag;
      btn.addEventListener('click',()=>{
        const selectedClass=isCompliment?'selected-positive':'selected-negative';
        btn.classList.toggle(selectedClass);
        playSound('click');
      });
      quickTagsEl.appendChild(btn);
    });

    const submitBtn=document.getElementById('feedback-submit-button');
    submitBtn.textContent=isCompliment?'שליחת מחמאה':'שליחת הערה';
    submitBtn.className='btn btn-full '+(isCompliment?'btn-green':'btn-red');

    document.getElementById('feedback-text').value='';
  }

  function renderReportsScreen(){
    const userName=getDisplayNameForUser(appState.currentUser);
    const userEntries=feedbackEntries.filter(f=>f.user_name===userName);

    const totalCompliments=userEntries.filter(f=>f.type==='compliment').length;
    const totalRemarks=userEntries.filter(f=>f.type==='remark').length;

    document.getElementById('reports-total-compliments').textContent=totalCompliments;
    document.getElementById('reports-total-remarks').textContent=totalRemarks;

    const weekAgo=new Date(Date.now()-7*24*60*60*1000);
    const weekEntries=userEntries.filter(f=>new Date(f.created_at)>=weekAgo);
    const weekCompliments=weekEntries.filter(f=>f.type==='compliment').length;
    const weekRemarks=weekEntries.filter(f=>f.type==='remark').length;
    document.getElementById('reports-week-compliments').textContent=weekCompliments;
    document.getElementById('reports-week-remarks').textContent=weekRemarks;

    const summary=document.getElementById('reports-week-summary-text');
    if(weekEntries.length===0) summary.textContent='אין נתונים לשבוע הזה.';
    else if(weekCompliments>weekRemarks) summary.textContent='שבוע חיובי – יותר מחמאות מהערות.';
    else if(weekRemarks>weekCompliments) summary.textContent='שבוע ביקורתי – יותר הערות ממחמאות.';
    else summary.textContent='איזון מושלם בין מחמאות להערות.';

    const latestList=document.getElementById('reports-latest-list');
    const empty=document.getElementById('reports-empty');
    latestList.innerHTML='';
    if(userEntries.length===0){
      empty.style.display='block';
      latestList.style.display='none';
      return;
    }
    empty.style.display='none';
    latestList.style.display='flex';

    userEntries
      .sort((a,b)=>new Date(b.created_at)-new Date(a.created_at))
      .slice(0,5)
      .forEach(e=>{
        const teacher=getTeacherById(e.teacher_id);
        const item=document.createElement('div');
        item.className='feedback-item';
        item.innerHTML=`
          <div>${e.text || '<i>אין טקסט</i>'}</div>
          <div class="feedback-tags">
            ${(e.tags||[]).map(tag=>`<span class="tag-pill">${tag}</span>`).join('')}
          </div>
          <div class="feedback-meta-line">
            <span>${e.type==='compliment'?'👍':'⚠️'} ${teacher?.name||'מורה'}</span>
            <span>${formatDateShort(e.created_at)}</span>
          </div>
        `;
        latestList.appendChild(item);
      });
  }

  function renderLeaderboardScreen(){
    const tbody=document.getElementById('leaderboard-body');
    tbody.innerHTML='';

    const entries=Object.keys(studentStats).map(name=>{
      const st=studentStats[name];
      const total=st.compliments+st.remarks;
      return { name, ...st, total };
    }).sort((a,b)=>b.total-a.total);

    if(entries.length===0){
      tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:18px;color:#9ca3af;">אין עדיין נתונים.</td></tr>';
      return;
    }
    const maxTotal=Math.max(...entries.map(e=>e.total));

    entries.forEach((e,idx)=>{
      const width=Math.max(8,(e.total/maxTotal)*100);
      const tr=document.createElement('tr');
      tr.innerHTML=`
        <td>${idx+1}</td>
        <td>${e.name}</td>
        <td>${e.compliments}</td>
        <td>${e.remarks}</td>
        <td>${e.total}</td>
        <td><div class="leaderboard-bar-wrapper"><div class="leaderboard-bar" style="width:${width}%"></div></div></td>
      `;
      tbody.appendChild(tr);
    });
  }

  function renderAdminScreen(){
    const container=document.getElementById('admin-teacher-list');
    container.innerHTML='';
    if(teachers.length===0){
      container.innerHTML='<div class="feedback-empty">אין מורים ברשימה.</div>';
      return;
    }
    teachers.forEach(t=>{
      const stats=getTeacherStats(t.id);
      const card=document.createElement('div');
      card.className='teacher-card';
      card.innerHTML=`
        <div class="teacher-main">
          <div class="teacher-name">${t.name}</div>
          <div class="teacher-subject">${t.subject}</div>
          <div style="font-size:12px;color:#9ca3af;">משובים: ${stats.compliments+stats.remarks}</div>
        </div>
        <div class="teacher-meta">
          <button class="btn btn-red btn-small" data-delete-id="${t.id}">
            <i class="fas fa-trash"></i><span>מחיקה</span>
          </button>
        </div>
      `;
      const btn=card.querySelector('[data-delete-id]');
      btn.addEventListener('click',async ev=>{
        ev.stopPropagation();
        if(confirm(`למחוק את המורה ${t.name}? כל המשובים עליו יימחקו.`)){
          const ok=await deleteTeacher(t.id);
          if(ok){
            renderAdminScreen();
            renderTeacherList();
          }else alert('הייתה בעיה במחיקה.');
        }
      });
      container.appendChild(card);
    });
    renderAdminFeedbackList();
  }

  function renderAdminFeedbackList(){
    const container=document.getElementById('admin-feedback-list');
    const search=(document.getElementById('admin-feedback-search')?.value || '').trim().toLowerCase();
    const filtered=feedbackEntries.filter(f=>{
      if(!search) return true;
      const teacher=getTeacherById(f.teacher_id);
      const teacherName=(teacher?.name||'').toLowerCase();
      const userName=(f.user_name||'').toLowerCase();
      const text=(f.text||'').toLowerCase();
      return teacherName.includes(search)||userName.includes(search)||text.includes(search);
    });
    container.innerHTML='';
    if(filtered.length===0){
      document.getElementById('admin-feedback-empty').style.display='block';
      container.style.display='none';
      return;
    }
    document.getElementById('admin-feedback-empty').style.display='none';
    container.style.display='flex';

    filtered.slice(0,30).forEach(e=>{
      const teacher=getTeacherById(e.teacher_id);
      const item=document.createElement('div');
      item.className='feedback-item';
      item.innerHTML=`
        <div><strong>${teacher?.name||'מורה'} – ${e.type==='compliment'?'מחמאה':'הערה'}</strong></div>
        <div>${e.text || '<i>אין טקסט</i>'}</div>
        <div class="feedback-tags">
          ${(e.tags||[]).map(tag=>`<span class="tag-pill">${tag}</span>`).join('')}
        </div>
        <div class="feedback-meta-line">
          <span>מאת: ${e.user_name}</span>
          <span>${formatDateShort(e.created_at)}</span>
        </div>
        <div class="feedback-actions">
          <button class="delete-feedback-btn" data-feedback-id="${e.id}">
            <i class="fas fa-trash"></i> מחק משוב
          </button>
        </div>
      `;
      const btn=item.querySelector('.delete-feedback-btn');
      btn.addEventListener('click',async ()=>{
        if(confirm('למחוק את המשוב?')){
          const ok=await deleteFeedback(e.id);
          if(ok){
            renderAdminFeedbackList();
            if(appState.currentScreen==='teacher-profile') renderTeacherProfile();
          }
        }
      });
      container.appendChild(item);
    });
  }

  // ---------- Smart Notifications ----------
  function updateNotifications(){
    const panelList=document.getElementById('notif-list');
    const badge=document.getElementById('notif-badge');
    if(!panelList || !badge || !appState.currentUser){ 
      if(badge) badge.style.display='none';
      return;
    }

    panelList.innerHTML='';
    const notes=[];
    const userName=getDisplayNameForUser(appState.currentUser);
    const userEntries=feedbackEntries.filter(f=>f.user_name===userName);

    if(userEntries.length===0){
      notes.push({
        type:'info',
        title:'עדיין לא כתבת משוב',
        text:'נסה לכתוב מחמאה קטנה למורה אחד היום. זה גם טוב לך וגם לו.'
      });
    }else{
      const compliments=userEntries.filter(f=>f.type==='compliment').length;
      const remarks=userEntries.filter(f=>f.type==='remark').length;

      if(compliments>remarks){
        notes.push({
          type:'good',
          title:'היחס שלך יותר חיובי',
          text:`כתבת עד עכשיו ${compliments} מחמאות ו-${remarks} הערות. יפה שאתה גם יודע לפרגן.`
        });
      }else if(remarks>compliments && remarks>=3){
        notes.push({
          type:'bad',
          title:'הרבה הערות לאחרונה',
          text:`יש לך יותר הערות ממחמאות. אולי שווה לנסות גם לכתוב מה כן עובד בשיעור.`
        });
      }

      const last=userEntries[0];
      if(last){
        const teacher=getTeacherById(last.teacher_id);
        notes.push({
          type:'info',
          title:'המשוב האחרון שנרשם',
          text:`כתבת ${last.type==='compliment'?'מחמאה':'הערה'} על ${teacher?.name||'מורה'} בתאריך ${formatDateShort(last.created_at)}.`
        });
      }
    }

    // מי המורה הכי מדורג כללית
    if(teachers.length && feedbackEntries.length){
      let best=null;
      teachers.forEach(t=>{
        const st=getTeacherStats(t.id);
        if(!best || st.score>best.score) best={teacher:t,score:st.score};
      });
      if(best && best.score>0){
        notes.push({
          type:'good',
          title:'מורה מוביל במערכת',
          text:`כרגע המורה עם הניקוד הכי גבוה הוא ${best.teacher.name} (ניקוד ${best.score}).`
        });
      }
    }

    if(notes.length===0){
      panelList.innerHTML='<div class="notif-empty">אין התראות כרגע.</div>';
      badge.style.display='none';
      return;
    }

    notes.forEach(n=>{
      const item=document.createElement('div');
      item.className='notif-item';
      const iconClass = n.type==='good' ? 'good' : n.type==='bad' ? 'bad' : 'info';
      const icon      = n.type==='good' ? 'fas fa-arrow-up'
                        : n.type==='bad' ? 'fas fa-exclamation'
                        : 'fas fa-info';
      item.innerHTML=`
        <div class="notif-icon ${iconClass}"><i class="${icon}"></i></div>
        <div>
          <div class="notif-title">${n.title}</div>
          <div class="notif-text">${n.text}</div>
        </div>
      `;
      panelList.appendChild(item);
    });

    badge.textContent=notes.length;
    badge.style.display='flex';
  }

  // ---------- Events ----------
  document.addEventListener('DOMContentLoaded', async ()=>{
    await loadData();
    setupRealtimeUpdates();
    autoLoginUser();

    document.getElementById('login-button').addEventListener('click',handleLogin);
    document.getElementById('login-password').addEventListener('keydown',e=>{
      if(e.key==='Enter') handleLogin();
    });

    document.getElementById('user-chip').addEventListener('click',()=>{
      if(appState.currentUser && confirm('להתנתק?')) handleLogout();
    });

    document.querySelectorAll('.tile').forEach(tile=>{
      tile.addEventListener('click',()=>{
        const action=tile.dataset.action;
        playSound('click');
        if(action==='view-teachers') showScreen('teachers');
        else if(action==='quick-compliment'){
          appState.feedbackType='compliment'; showScreen('teachers');
        }else if(action==='quick-remark'){
          appState.feedbackType='remark'; showScreen('teachers');
        }else if(action==='view-reports') showScreen('reports');
        else if(action==='leaderboard') showScreen('leaderboard');
        else if(action==='admin'){
          if(appState.currentUser?.role==='admin') showScreen('admin');
          else alert('רק אדמין יכול להיכנס לכאן.');
        }
      });
    });

    document.querySelectorAll('.back-link').forEach(link=>{
      link.addEventListener('click',()=>{
        const target=link.dataset.backTo;
        if(target){ playSound('click'); showScreen(target); }
      });
    });

    document.getElementById('teacher-search').addEventListener('input',renderTeacherList);
    document.getElementById('admin-feedback-search')?.addEventListener('input',renderAdminFeedbackList);

    document.getElementById('btn-profile-compliment').addEventListener('click',()=>{
      appState.feedbackType='compliment'; showScreen('feedback');
    });
    document.getElementById('btn-profile-remark').addEventListener('click',()=>{
      appState.feedbackType='remark'; showScreen('feedback');
    });

    document.getElementById('feedback-back').addEventListener('click',()=>{
      playSound('click');
      showScreen('teacher-profile');
    });

    document.getElementById('feedback-submit-button').addEventListener('click',async ()=>{
      const teacherId=appState.selectedTeacherId;
      const type=appState.feedbackType;
      if(!teacherId || !type){ showErrorAnimation(); return; }

      const teacher=getTeacherById(teacherId);
      const text=document.getElementById('feedback-text').value.trim();

      const quickWrap=document.getElementById('quick-tags');
      const tagButtons=[...quickWrap.querySelectorAll('.quick-tag')];
      const tags=tagButtons.filter(btn =>
        btn.classList.contains('selected-positive') ||
        btn.classList.contains('selected-negative')
      ).map(btn=>btn.textContent);

      if(tags.length===0 && !text){
        showErrorAnimation();
        return;
      }

      const userName=getDisplayNameForUser(appState.currentUser);
      const saved=await saveFeedback({
        teacherId, type, tags, text, user:userName
      });

      if(saved){
        playSound('send');
        showFeedbackAnimation(type, teacher.name);
        renderTeacherProfile();
        renderReportsScreen();
        renderLeaderboardScreen();

        setTimeout(()=>{ showScreen('teacher-profile'); },900);
      }else{
        showErrorAnimation();
      }
    });

    document.getElementById('admin-add-teacher').addEventListener('click',async ()=>{
      const nameInput=document.getElementById('admin-new-name');
      const subjectInput=document.getElementById('admin-new-subject');
      const name=nameInput.value.trim();
      const subject=subjectInput.value.trim();
      if(!name || !subject){
        alert('צריך למלא גם שם וגם מקצוע.');
        return;
      }
      const newTeacher=await addTeacher(name,subject);
      if(newTeacher){
        nameInput.value='';subjectInput.value='';
        renderAdminScreen();renderTeacherList();
        playSound('send');
        alert('המורה נוסף בהצלחה!');
      }else alert('הייתה בעיה בהוספת המורה.');
    });

    document.getElementById('help-button').addEventListener('click',()=>{
      alert('SchoolRank – מערכת משוב למורים\n\n• אפשר להיכנס עם כל שם משתמש\n• adir / 1234 נותן הרשאות אדמין\n• משובים נשמרים ב-Supabase ונראים לכולם\n• המערכת זוכרת אותך אוטומטית בעזרת קוקיז.');
    });

    document.getElementById('notif-button').addEventListener('click',()=>{
      const panel=document.getElementById('notif-panel');
      panel.classList.toggle('open');
      playSound('notif');
    });
  });
</script>
</body>
</html>
