<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>SchoolRank | מערכת משוב</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Rubik:wght@400;500;700;900&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    :root {
      --primary: #4f46e5;
      --success: #10b981;
      --danger: #ef4444;
      --dark-bg: #0b0f19;
      --card-bg: rgba(30, 41, 59, 0.65);
      --glass-border: rgba(255, 255, 255, 0.1);
      --text-main: #f8fafc;
    }

    * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; user-select: none; }

    body {
      margin: 0;
      font-family: 'Rubik', sans-serif;
      background-color: var(--dark-bg);
      color: var(--text-main);
      overflow-x: hidden;
      min-height: 100vh;
    }

    /* רקע חי וזז */
    .bg-animate {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1;
      background: radial-gradient(circle at 50% 50%, #1e293b, #000);
      overflow: hidden;
    }
    .orb {
      position: absolute; border-radius: 50%; filter: blur(80px); opacity: 0.4;
      animation: floatOrb 10s infinite alternate ease-in-out;
    }
    .orb-1 { width: 400px; height: 400px; background: #4f46e5; top: -100px; left: -100px; }
    .orb-2 { width: 300px; height: 300px; background: #ec4899; bottom: -50px; right: -50px; animation-delay: -2s; }
    @keyframes floatOrb { from { transform: translate(0,0); } to { transform: translate(30px, 40px); } }

    /* Glass UI */
    .glass {
      background: var(--card-bg);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid var(--glass-border);
      box-shadow: 0 8px 32px rgba(0,0,0,0.3);
    }

    .app { 
      max-width: 1200px;
      margin: 0 auto; 
      min-height: 100vh; 
      display: flex; 
      flex-direction: column; 
    }

    /* Header */
    header {
      padding: 20px; display: flex; justify-content: space-between; align-items: center;
      background: rgba(11, 15, 25, 0.8); backdrop-filter: blur(10px); z-index: 50;
      border-bottom: 1px solid var(--glass-border);
      border-radius: 0 0 20px 20px;
      margin-bottom: 20px;
    }
    .logo { 
      font-weight: 900; 
      font-size: 24px; 
      background: linear-gradient(90deg, #ffffff, #94a3b8); 
      -webkit-background-clip: text; 
      -webkit-text-fill-color: transparent; 
    }

    /* אזור ימין של ההדר */
    .header-right {
      display:flex;
      align-items:center;
      gap:12px;
    }

    /* כפתור התנתקות */
    .btn-logout {
      padding: 6px 12px;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.6);
      background: rgba(15,23,42,0.7);
      color: #e5e7eb;
      font-size: 12px;
      cursor: pointer;
      display:flex;
      align-items:center;
      gap:6px;
      transition: 0.15s;
    }
    .btn-logout:hover {
      background: rgba(30,64,175,0.8);
      border-color: rgba(129,140,248,0.9);
    }
    .btn-logout:active {
      transform: scale(0.96);
    }

    /* Screens */
    .screen { display: none; padding: 20px; flex: 1; overflow-y: auto; animation: slideUp 0.4s cubic-bezier(0.2, 0.8, 0.2, 1); }
    .screen.active { display: block; }
    @keyframes slideUp { from { opacity: 0; transform: translateY(20px) scale(0.98); } to { opacity: 1; transform: translateY(0) scale(1); } }

    .menu-grid { 
      display: grid; 
      grid-template-columns: repeat(3, 1fr);
      gap: 20px; 
      margin-top: 20px; 
    }

    @media (max-width: 900px) {
      .menu-grid { grid-template-columns: 1fr; }
      .app { max-width: 100%; }
    }

    .card-btn {
      padding: 24px; 
      border-radius: 16px; 
      cursor: pointer;
      transition: all 0.2s; 
      position: relative; 
      overflow: hidden;
      display: flex;
      align-items: flex-start;
      gap: 20px;
      text-align: right;
      background: rgba(30, 41, 59, 0.7);
      border: 1px solid rgba(255,255,255,0.05);
      min-height: 150px;
    }

    .card-btn:hover { 
      transform: translateY(-5px); 
      background: rgba(30, 41, 59, 0.95);
      border-color: rgba(255,255,255,0.2);
      box-shadow: 0 10px 30px rgba(0,0,0,0.3);
    }
    
    .card-btn:active { transform: scale(0.98); }

    .card-icon-box {
      width: 50px; height: 50px;
      border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      font-size: 24px; color: white;
      flex-shrink: 0;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }

    .card-content h3 { margin: 0 0 10px 0; font-size: 20px; color: #fff; font-weight: 700; }
    .card-content span { font-size: 14px; color: #94a3b8; line-height: 1.5; display: block; }

    input, textarea {
      width: 100%; background: rgba(0,0,0,0.3); border: 1px solid var(--glass-border);
      border-radius: 12px; padding: 14px; color: #fff; font-family: inherit; font-size: 16px;
      margin-bottom: 12px; outline: none; transition: 0.3s;
    }
    input:focus, textarea:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.2); }

    .btn {
      width: 100%; padding: 16px; border-radius: 14px; border: none; font-weight: 700; font-size: 16px;
      cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px;
      transition: 0.2s; color: white;
    }
    .btn-primary { background: linear-gradient(135deg, #4f46e5, #6366f1); box-shadow: 0 4px 15px rgba(79, 70, 229, 0.4); }
    .btn-success { background: linear-gradient(135deg, #10b981, #059669); box-shadow: 0 4px 15px rgba(16, 185, 129, 0.4); }
    .btn-danger { background: linear-gradient(135deg, #ef4444, #dc2626); box-shadow: 0 4px 15px rgba(239, 68, 68, 0.4); }
    .btn:active { transform: scale(0.95); }

    .t-item {
      display: flex; align-items: center; justify-content: space-between; padding: 16px;
      border-radius: 16px; margin-bottom: 10px; cursor: pointer; transition: 0.2s;
    }
    .t-item:hover { background: rgba(255,255,255,0.1); transform: translateX(-5px); }
    .avatar {
      width: 44px; height: 44px; border-radius: 12px; display: flex; align-items: center; justify-content: center;
      font-weight: 700; font-size: 18px; margin-left: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    }

    .tags-wrap { display: flex; flex-wrap: wrap; gap: 8px; margin: 15px 0; }
    .tag {
      padding: 8px 16px; border-radius: 20px; font-size: 14px; cursor: pointer;
      border: 1px solid var(--glass-border); background: rgba(255,255,255,0.05); transition: 0.2s;
    }
    .tag.active-pos { background: var(--success); border-color: var(--success); transform: scale(1.05); box-shadow: 0 0 15px rgba(16,185,129,0.5); }
    .tag.active-neg { background: var(--danger); border-color: var(--danger); transform: scale(1.05); box-shadow: 0 0 15px rgba(239,68,68,0.5); }

    @keyframes shakeScreen {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-10px) rotate(-1deg); }
      75% { transform: translateX(10px) rotate(1deg); }
    }
    .shake-fx { animation: shakeScreen 0.4s cubic-bezier(.36,.07,.19,.97) both; }
    
    .overlay-flash {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 100;
      opacity: 0; transition: opacity 0.5s;
    }

    .particle {
      position: fixed; pointer-events: none; z-index: 9999;
      animation: flyOut 1s ease-out forwards; font-size: 24px;
    }
    @keyframes flyOut {
      0% { transform: translate(-50%, -50%) scale(0.5); opacity: 1; }
      100% { transform: translate(var(--tx), var(--ty)) scale(1.5) rotate(var(--rot)); opacity: 0; }
    }
    
    .login-container {
        max-width: 400px; margin: 0 auto; text-align: center;
    }

    .form-field { margin-bottom: 16px; }
    .form-label {
      font-size: 13px;
      margin-bottom: 6px;
      color: #94a3b8;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .text-input {
      width: 100%;
      padding: 10px 14px;
      border-radius: 12px;
      border: 1px solid rgba(148,163,184,0.4);
      background: rgba(15,23,42,0.6);
      color: #e5e7eb;
      outline: none;
      font-size: 14px;
      transition: all 0.2s ease;
    }
    .text-input:focus {
      border-color: #22c55e;
      box-shadow: 0 0 0 2px rgba(34,197,94,0.3);
    }

    /* דוחות – סטייל */
    .reports-section {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
      gap: 16px;
      margin-top: 10px;
    }
    .report-summary-card {
      padding: 18px;
      font-size: 14px;
    }
    .report-row {
      display: flex;
      justify-content: space-between;
      margin-bottom: 6px;
      color: #e5e7eb;
    }
    .report-highlight {
      margin-top: 10px;
      font-size: 13px;
      background: rgba(56,189,248,0.1);
      border-radius: 8px;
      padding: 8px 10px;
      color: #7dd3fc;
    }
    .feedback-list {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .feedback-empty {
      text-align: center;
      color: #94a3b8;
      font-size: 13px;
      padding-top: 10px;
    }
  </style>
</head>
<body>

  <div class="bg-animate">
    <div class="orb orb-1"></div>
    <div class="orb orb-2"></div>
  </div>
  <div id="flash-fx" class="overlay-flash"></div>

  <div class="app">
    <header>
      <div class="logo">SchoolRank</div>
      <div class="header-right">
        <div id="user-chip" style="font-size:14px; color:#cbd5e1; display:flex; align-items:center; gap:10px;">
          <i class="fas fa-user-circle" style="font-size:20px;"></i> 
          <span>אורח</span>
        </div>
        <button id="logout-btn" class="btn-logout" onclick="app.logout()" style="display:none;">
          <i class="fas fa-sign-out-alt"></i>
          <span>התנתקות</span>
        </button>
      </div>
    </header>

    <!-- לוגין -->
    <section id="s-login" class="screen active">
      <div class="login-container">
        <h1 style="font-size:32px; margin-bottom:10px;">כניסה למערכת</h1>
        <p style="color:#94a3b8; margin-bottom:30px;">השפעה אמיתית על השיעור הבא שלך.</p>
        
        <div class="glass" style="padding:24px; border-radius:24px; text-align:right;">
          
          <div class="form-field">
            <div class="form-label">
              <i class="fas fa-user"></i>
              <span>שם משתמש</span>
            </div>
            <input type="text" id="login-username" class="text-input" placeholder="לדוגמה: תלמיד">
          </div>

          <div class="form-field">
            <div class="form-label">
              <i class="fas fa-lock"></i>
              <span>סיסמה</span>
            </div>
            <input type="password" id="login-password" class="text-input" placeholder="אפשר כל דבר">
          </div>

          <button class="btn btn-primary" id="login-button" style="width:100%; margin-top:10px;">
            התחברות <i class="fas fa-sign-in-alt"></i>
          </button>

          <div style="margin-top: 16px; padding: 12px; background: rgba(59, 130, 246, 0.1); border-radius: 8px; border: 1px solid rgba(59, 130, 246, 0.3);">
            <div style="font-size: 12px; color: #93c5fd; display: flex; align-items: center; gap: 6px; margin-bottom:4px;">
              <i class="fas fa-cloud"></i>
              <span>המערכת מחוברת ל-Supabase - כל המשובים נשמרים ונראים לכל המשתמשים!</span>
            </div>
            <div id="sync-status" style="font-size: 12px; color: #93c5fd; display:flex; align-items:center; gap:6px;">
              <i class="fas fa-database"></i>
              <span>מחובר - נתונים משותפים</span>
            </div>
          </div>

        </div>
      </div>
    </section>

    <!-- דשבורד -->
    <section id="s-home" class="screen">
      <div style="text-align:right; margin-bottom:30px;">
        <h1 style="font-size:36px; margin-bottom:5px;">מה אתה רוצה לעשות היום?</h1>
        <p style="color:#94a3b8; font-size:18px;">מערכת משוב. לא מושלמת, אבל לפחות היא בצד שלך.</p>
      </div>
      
      <div class="menu-grid">
        
        <div class="card-btn" onclick="app.nav('teachers')" onmouseenter="sfx.hover()">
          <div class="card-icon-box" style="background: #0f766e;">
            <i class="fas fa-users"></i>
          </div>
          <div class="card-content">
            <h3>צפייה במורים</h3>
            <span>לעבור על כל המורים, לראות מי זוכה בנקודות.</span>
          </div>
        </div>

        <div class="card-btn" onclick="app.nav('teachers', 'compliment')" onmouseenter="sfx.hover()">
          <div class="card-icon-box" style="background: #10b981;">
            <i class="fas fa-heart"></i>
          </div>
          <div class="card-content">
            <h3>הוספת מחמאה</h3>
            <span>הסביר ברור, היה נחמד, היה אנושי – מגיע לו.</span>
          </div>
        </div>

        <div class="card-btn" onclick="app.nav('teachers', 'remark')" onmouseenter="sfx.hover()">
          <div class="card-icon-box" style="background: #ef4444;">
            <i class="fas fa-exclamation-triangle"></i>
          </div>
          <div class="card-content">
            <h3>הוספת הערה</h3>
            <span>לא ברור, לא הוגן, או פשוט לא. כותבים את זה.</span>
          </div>
        </div>

        <div class="card-btn" onclick="app.nav('reports')" onmouseenter="sfx.hover()">
          <div class="card-icon-box" style="background: #3b82f6;">
            <i class="fas fa-chart-bar"></i>
          </div>
          <div class="card-content">
            <h3>הדוחות שלי</h3>
            <span>כל מה שכבר כתבת – טוב ורע – במקום אחד.</span>
          </div>
        </div>

        <div class="card-btn" onclick="app.nav('leaderboard')" onmouseenter="sfx.hover()">
          <div class="card-icon-box" style="background: #6366f1;">
            <i class="fas fa-trophy"></i>
          </div>
          <div class="card-content">
            <h3>לוח מדרגים</h3>
            <span>מי הכי פעיל במתן משובים בבית הספר?</span>
          </div>
        </div>

        <div class="card-btn" id="btn-admin" style="display:none;" onclick="app.nav('admin')" onmouseenter="sfx.hover()">
          <div class="card-icon-box" style="background: #a855f7;">
            <i class="fas fa-cog"></i>
          </div>
          <div class="card-content">
            <h3>ניהול מורים (אדמין)</h3>
            <span>הוספת מורים, מחיקה וניהול רשימה.</span>
          </div>
        </div>

      </div>
    </section>

    <!-- רשימת מורים -->
    <section id="s-teachers" class="screen">
      <div style="max-width:800px; margin:0 auto;">
        <button class="btn" style="background:transparent; justify-content:flex-start; padding:0; margin-bottom:20px; color:#94a3b8;" onclick="app.nav('home')">
          <i class="fas fa-arrow-right"></i> חזרה לדשבורד
        </button>
        <h2 style="margin-bottom:20px;">בחר מורה מהרשימה</h2>
        <input type="text" id="search-t" placeholder="חפש מורה..." oninput="app.renderTeachers()">
        <div id="list-t"></div>
      </div>
    </section>

    <!-- מסך משוב -->
    <section id="s-feedback" class="screen">
      <div class="glass" style="max-width:600px; margin:0 auto; padding:30px; border-radius:24px; text-align:center;">
        <div id="fb-avatar" class="avatar" style="width:80px; height:80px; font-size:32px; margin:0 auto 15px;"></div>
        <h2 id="fb-name" style="margin:0;">שם המורה</h2>
        <div id="fb-mode-label" style="margin-bottom:20px; font-size:16px; opacity:0.7; margin-top:5px;"></div>

        <div id="fb-tags" class="tags-wrap" style="justify-content:center;"></div>
        
        <textarea id="fb-text" rows="4" placeholder="פירוט (לא חובה)..."></textarea>
        
        <button id="fb-submit" class="btn" style="margin-top:10px;">שלח משוב</button>
        <button class="btn" style="background:transparent; color:#94a3b8; margin-top:10px;" onclick="app.nav('teachers')">ביטול</button>
      </div>
    </section>

    <!-- דוחות -->
    <section id="s-reports" class="screen">
      <div style="max-width:800px; margin:0 auto;">
        <button class="btn" style="background:transparent; justify-content:flex-start; padding:0; margin-bottom:20px; color:#94a3b8;" onclick="app.nav('home')">
          <i class="fas fa-arrow-right"></i> חזרה
        </button>

        <h2 style="margin-bottom:20px;">הדוחות שלי</h2>

        <div class="reports-section">
          <div class="report-summary-card glass" style="border-radius:20px;">
            <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
              <i class="fas fa-chart-pie"></i>
              <strong>סיכום כללי</strong>
            </div>
            <div class="report-row">
              <span>סה״כ מחמאות</span>
              <span id="reports-total-compliments">0</span>
            </div>
            <div class="report-row">
              <span>סה״כ הערות</span>
              <span id="reports-total-remarks">0</span>
            </div>
          </div>

          <div class="report-summary-card glass" style="border-radius:20px;">
            <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
              <i class="fas fa-calendar-week"></i>
              <strong>סיכום שבועי</strong>
            </div>
            <div class="report-row">
              <span>מחמאות בשבוע האחרון</span>
              <span id="reports-week-compliments">0</span>
            </div>
            <div class="report-row">
              <span>הערות בשבוע האחרון</span>
              <span id="reports-week-remarks">0</span>
            </div>
            <div class="report-highlight" id="reports-week-summary-text">
              עדיין אין נתונים לשבוע הזה.
            </div>
          </div>

          <div class="report-summary-card glass" style="border-radius:20px;">
            <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
              <i class="fas fa-clock"></i>
              <strong>פעילות אחרונה</strong>
            </div>
            <div id="reports-latest-list" class="feedback-list"></div>
            <div id="reports-empty" class="feedback-empty">
              <i class="fas fa-inbox"></i>
              <div>עדיין אין משובים.</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- לוח מדרגים -->
    <section id="s-leaderboard" class="screen">
      <div style="max-width:800px; margin:0 auto;">
        <button class="btn" style="background:transparent; justify-content:flex-start; padding:0; margin-bottom:20px; color:#94a3b8;" onclick="app.nav('home')">
          <i class="fas fa-arrow-right"></i> חזרה
        </button>
        <h2>🏆 טבלת האלופים</h2>
        <div id="leaderboard-list" class="glass" style="border-radius:20px; overflow:hidden;"></div>
      </div>
    </section>

    <!-- אדמין -->
    <section id="s-admin" class="screen">
      <div style="max-width:800px; margin:0 auto;">
        <button class="btn" style="background:transparent; justify-content:flex-start; padding:0; margin-bottom:20px; color:#94a3b8;" onclick="app.nav('home')">
          <i class="fas fa-arrow-right"></i> חזרה
        </button>

        <!-- חלק 1: ניהול מורים -->
        <h2>ניהול מורים</h2>
        <div class="glass" style="padding:20px; border-radius:20px; margin-bottom:20px;">
          <input id="new-t-name" placeholder="שם מורה חדש">
          <input id="new-t-sub" placeholder="מקצוע">
          <button class="btn btn-success" onclick="app.addTeacher()">הוסף מורה</button>
        </div>
        <div id="admin-list" style="margin-bottom:30px;"></div>

        <!-- חלק 2: ניהול משובים -->
        <h2 style="margin-bottom:10px;">ניהול משובים</h2>
        <p style="color:#94a3b8; font-size:14px; margin-top:0; margin-bottom:10px;">
          כאן אפשר למחוק מחמאות והערות (למשל אם נשלחו בטעות או לא מכבדות).
        </p>
        <div id="admin-feedback-list" class="glass" style="padding:16px; border-radius:20px; max-height:400px; overflow-y:auto;">
        </div>
      </div>
    </section>

  </div>

<script>
// --- SOUND ENGINE ---
const sfx = {
  ctx: null,
  init: () => {
    window.AudioContext = window.AudioContext || window.webkitAudioContext;
    sfx.ctx = new AudioContext();
  },
  playTone: (freq, type, duration, vol = 0.1) => {
    if (!sfx.ctx) sfx.init();
    const osc = sfx.ctx.createOscillator();
    const gain = sfx.ctx.createGain();
    osc.type = type;
    osc.frequency.setValueAtTime(freq, sfx.ctx.currentTime);
    gain.gain.setValueAtTime(vol, sfx.ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.01, sfx.ctx.currentTime + duration);
    osc.connect(gain);
    gain.connect(sfx.ctx.destination);
    osc.start();
    osc.stop(sfx.ctx.currentTime + duration);
  },
  hover: () => sfx.playTone(400, 'sine', 0.1, 0.02),
  click: () => sfx.playTone(600, 'triangle', 0.1, 0.05),
  select: () => {
    sfx.playTone(800, 'sine', 0.1, 0.05);
    setTimeout(() => sfx.playTone(1200, 'sine', 0.2, 0.03), 50);
  },
  success: () => {
    if (!sfx.ctx) sfx.init();
    [523.25, 659.25, 783.99, 1046.50].forEach((f, i) => { 
      setTimeout(() => sfx.playTone(f, 'sine', 0.6, 0.1), i * 80);
    });
    setTimeout(() => sfx.playTone(2000, 'triangle', 0.8, 0.05), 400);
  },
  remark: () => {
    sfx.playTone(150, 'sawtooth', 0.4, 0.1);
    sfx.playTone(100, 'square', 0.5, 0.1);
    setTimeout(() => sfx.playTone(80, 'sine', 0.6, 0.2), 100);
  }
};

const SUPABASE_URL = 'https://xidfthnboggokcsloglt.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhpZGZ0aG5ib2dnb2tjc2xvZ2x0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjQzMTIwNzUsImV4cCI6MjA3OTg4ODA3NX0.fEPS2FJYlcZ4DOv7I0RBEcwZBfT0MdRGslk9cp_2GwU';
const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

const app = {
  user: null,
  teachers: [],
  mode: null,
  selTeacher: null,

  init: async () => {
    const saved = localStorage.getItem('tf_user');
    if (saved) {
      app.user = JSON.parse(saved);
      app.updateUserChip();
      app.showScreen('home');
    } else {
      app.showScreen('login');
    }

    await app.fetchData();
    app.setupRealtime();
    app.updateLeaderboard();
  },

  updateUserChip: () => {
    const chip = document.querySelector('#user-chip span');
    const logoutBtn = document.getElementById('logout-btn');
    if (app.user && chip) {
      const roleLabel = app.user.role === 'admin' ? 'אדמין' : 'תלמיד';
      chip.innerText = `מחובר: ${roleLabel} (${app.user.name})`;
      if (logoutBtn) logoutBtn.style.display = 'flex';
    } else {
      if (chip) chip.innerText = 'אורח';
      if (logoutBtn) logoutBtn.style.display = 'none';
    }
  },

  logout: () => {
    app.user = null;
    localStorage.removeItem('tf_user');
    app.mode = null;
    app.selTeacher = null;
    app.updateUserChip();
    sfx.click();
    app.showScreen('login');
  },

  fetchData: async () => {
    let { data } = await supabase.from('teachers').select('*');
    app.teachers = data || [];
  },
  
  setupRealtime: () => {
    supabase
      .channel('public:feedback')
      .on(
        'postgres_changes',
        { event: '*', schema: 'public', table: 'feedback' },
        () => {
          app.updateLeaderboard();
          if (app.user) app.renderReports();
          app.renderTeachers();
          app.renderAdmin();
        }
      )
      .subscribe();
  },

  nav: (scr, mode) => {
    sfx.click();

    if (mode) {
      app.mode = mode;
    } else if (scr === 'teachers') {
      app.mode = null;
    }

    app.showScreen(scr);
  },

  showScreen: (id) => {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    const scr = document.getElementById('s-' + id);
    if (scr) scr.classList.add('active');
  
    const header = document.querySelector('header');
    const logoutBtn = document.getElementById('logout-btn');
    if (header) {
      if (id === 'login') {
        header.style.display = 'none';
      } else {
        header.style.display = 'flex';
      }
    }
    if (logoutBtn) {
      logoutBtn.style.display = (id === 'login' || !app.user) ? 'none' : 'flex';
    }

    if (id === 'home') {
      app.updateUserChip();
      const adminBtn = document.getElementById('btn-admin');
      if (adminBtn) adminBtn.style.display = app.user && app.user.role === 'admin' ? 'flex' : 'none';
      app.updateLeaderboard();
    }

    if (id === 'teachers') app.renderTeachers();
    if (id === 'reports') app.renderReports();
    if (id === 'admin') app.renderAdmin();
  },

  renderTeachers: async () => {
    const list = document.getElementById('list-t');
    const termEl = document.getElementById('search-t');
    const term = termEl ? termEl.value.toLowerCase() : '';
    if (!list) return;
    list.innerHTML = '';

    let teachers = app.teachers;
    if (!teachers || teachers.length === 0) {
      let { data } = await supabase.from('teachers').select('*');
      teachers = data || [];
      app.teachers = teachers;
    }

    let { data: feed } = await supabase
      .from('feedback')
      .select('teacher_id, type');

    const statsMap = {};
    (feed || []).forEach(f => {
      if (!statsMap[f.teacher_id]) {
        statsMap[f.teacher_id] = { compliments: 0, remarks: 0 };
      }
      if (f.type === 'compliment') statsMap[f.teacher_id].compliments++;
      if (f.type === 'remark') statsMap[f.teacher_id].remarks++;
    });

    const filtered = teachers.filter(t =>
      !term || t.name.toLowerCase().includes(term)
    );

    if (filtered.length === 0) {
      list.innerHTML = `<div style="padding:20px; text-align:center; color:#94a3b8;">לא נמצאו מורים.</div>`;
      return;
    }

    filtered.forEach(t => {
      const s = statsMap[t.id] || { compliments: 0, remarks: 0 };
      const score = s.compliments - s.remarks;

      const d = document.createElement('div');
      d.className = 't-item glass';

      if (app.mode === 'compliment' || app.mode === 'remark') {
        d.onclick = () => app.prepFeedback(t);
      } else {
        d.onclick = null;
      }

      d.onmouseenter = sfx.hover;

      d.innerHTML = `
        <div style="display:flex; align-items:center; justify-content:space-between; gap:10px;">
          <div style="display:flex; align-items:center;">
            <div class="avatar" style="background:${stringToColor(t.name)}">${t.name[0]}</div>
            <div style="margin-right:15px;">
              <div style="font-weight:700;">${t.name}</div>
              <div style="font-size:12px; opacity:0.7;">${t.subject || ''}</div>
            </div>
          </div>
          <div style="text-align:left; font-size:12px;">
            <div style="margin-bottom:4px;">
              <span style="color:#22c55e;">❤ ${s.compliments}</span>
              &nbsp;&nbsp;
              <span style="color:#f97316;">⚠ ${s.remarks}</span>
            </div>
            <div style="opacity:0.7;">ניקוד: ${score > 0 ? '+'+score : score}</div>
          </div>
        </div>
      `;

      list.appendChild(d);
    });
  },

  prepFeedback: (t) => {
    sfx.select();
    app.selTeacher = t;
    app.showScreen('feedback');
    
    const isPos = app.mode === 'compliment';
    const av = document.getElementById('fb-avatar');
    av.style.background = stringToColor(t.name);
    av.innerText = t.name[0];
    document.getElementById('fb-name').innerText = t.name;
    document.getElementById('fb-mode-label').innerText = isPos ? 'על מה מגיע פרגון?' : 'מה צריך שיפור?';
    
    const tags = isPos 
      ? ['הסבר ברור', 'יחס אישי', 'שיעור כיף', 'עוזר בחומר', 'מצחיק']
      : ['לא מובן', 'יחס מזלזל', 'משעמם', 'צעקות', 'איחור'];
      
    const cont = document.getElementById('fb-tags');
    cont.innerHTML = '';
    tags.forEach(tag => {
      const el = document.createElement('div');
      el.className = 'tag';
      el.innerText = tag;
      el.onclick = function() {
        sfx.click();
        this.classList.toggle(isPos ? 'active-pos' : 'active-neg');
      };
      cont.appendChild(el);
    });
    
    const btn = document.getElementById('fb-submit');
    btn.className = `btn ${isPos ? 'btn-success' : 'btn-danger'}`;
    btn.innerHTML = isPos ? 'שלח פרגון <i class="fas fa-heart"></i>' : 'שלח הערה <i class="fas fa-paper-plane"></i>';
    btn.onclick = app.submitFeedback;
  },

  submitFeedback: async () => {
    if (!app.user) {
      alert('צריך להתחבר למערכת קודם');
      return;
    }

    const txt = document.getElementById('fb-text').value;
    const tags = Array.from(document.querySelectorAll('.tag.active-pos, .tag.active-neg')).map(e => e.innerText);
    
    if (!txt && tags.length === 0) {
      alert('בחר תגית או כתוב משהו');
      return;
    }
    
    const isPos = app.mode === 'compliment';
    
    if (isPos) {
      sfx.success();
      triggerConfetti('🎉', '❤️', '⭐');
      flashScreen('#10b981');
    } else {
      sfx.remark();
      triggerConfetti('⚠️', '❗', '⚡');
      flashScreen('#ef4444');
      document.body.classList.add('shake-fx');
      setTimeout(() => document.body.classList.remove('shake-fx'), 500);
    }
    
    await supabase.from('feedback').insert([{
      teacher_id: app.selTeacher.id,
      user_name: app.user.name,
      type: app.mode,
      tags: tags,
      text: txt
    }]);

    document.getElementById('fb-text').value = '';
    setTimeout(() => app.nav('home'), 1500); 
  },

  updateLeaderboard: async () => {
    let { data: feed } = await supabase.from('feedback').select('user_name');
    const counts = {};
    (feed || []).forEach(f => {
      if (!f.user_name) return;
      counts[f.user_name] = (counts[f.user_name] || 0) + 1;
    });
    const sorted = Object.entries(counts).sort((a,b) => b[1] - a[1]).slice(0, 10);
     
    const el = document.getElementById('leaderboard-list');
    if (!el) return;
    el.innerHTML = '';
    sorted.forEach((u, i) => {
      el.innerHTML += `
        <div style="display:flex; justify-content:space-between; padding:15px; border-bottom:1px solid rgba(255,255,255,0.05);">
          <div><span style="font-weight:bold; width:20px; display:inline-block;">#${i+1}</span> ${u[0]}</div>
          <div style="font-weight:bold; color:var(--primary)">${u[1]} נק'</div>
        </div>
      `;
    });
  },

  renderReports: async () => {
    if (!app.user || !app.user.name) return;

    let { data: reports } = await supabase.from('feedback')
      .select('created_at, type, tags, text, teachers(name)')
      .eq('user_name', app.user.name)
      .order('created_at', { ascending: false });

    reports = reports || [];

    const totalCompliments = reports.filter(r => r.type === 'compliment').length;
    const totalRemarks = reports.filter(r => r.type === 'remark').length;

    const elTotalPos = document.getElementById('reports-total-compliments');
    const elTotalNeg = document.getElementById('reports-total-remarks');
    if (elTotalPos) elTotalPos.textContent = totalCompliments;
    if (elTotalNeg) elTotalNeg.textContent = totalRemarks;

    const weekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000);
    const weekEntries = reports.filter(r => new Date(r.created_at) >= weekAgo);

    const weekCompliments = weekEntries.filter(r => r.type === 'compliment').length;
    const weekRemarks = weekEntries.filter(r => r.type === 'remark').length;

    const elWeekPos = document.getElementById('reports-week-compliments');
    const elWeekNeg = document.getElementById('reports-week-remarks');
    const elWeekText = document.getElementById('reports-week-summary-text');

    if (elWeekPos) elWeekPos.textContent = weekCompliments;
    if (elWeekNeg) elWeekNeg.textContent = weekRemarks;

    if (elWeekText) {
      if (weekEntries.length === 0) {
        elWeekText.textContent = 'עדיין אין נתונים לשבוע הזה.';
      } else if (weekCompliments > weekRemarks) {
        elWeekText.textContent = 'יותר מחמאות מהערות – מגמה חיובית 🌱';
      } else if (weekCompliments < weekRemarks) {
        elWeekText.textContent = 'יותר הערות ממחמאות – שווה לחשוב מה אפשר לשפר.';
      } else {
        elWeekText.textContent = 'איזון בין מחמאות להערות השבוע.';
      }
    }

    const listEl = document.getElementById('reports-latest-list');
    const emptyEl = document.getElementById('reports-empty');
    if (!listEl || !emptyEl) return;

    listEl.innerHTML = '';

    if (reports.length === 0) {
      emptyEl.style.display = 'block';
      return;
    }

    emptyEl.style.display = 'none';

    const latest = reports.slice(0, 5);
    latest.forEach(r => {
      const isPos = r.type === 'compliment';
      const typeColor = isPos ? 'var(--success)' : 'var(--danger)';
      const typeIcon = isPos ? 'fas fa-heart' : 'fas fa-bolt';
      const typeText = isPos ? 'פרגון' : 'הערה';
      const teacherName = r.teachers ? r.teachers.name : 'מורה לא ידוע';
      const date = new Date(r.created_at).toLocaleDateString('he-IL', {
        day: 'numeric',
        month: 'numeric',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });

      const tagsHtml = r.tags && r.tags.length > 0
        ? r.tags.map(tag => `
            <span style="background:${isPos ? 'rgba(16,185,129,0.2)' : 'rgba(239,68,68,0.2)'}; 
                         color:${typeColor}; padding:4px 8px; border-radius:10px; font-size:12px;">
              ${tag}
            </span>`).join(' ')
        : '';

      listEl.innerHTML += `
        <div style="padding: 10px 0; border-bottom: 1px solid rgba(148,163,184,0.2);">
          <div style="display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:4px;">
            <div style="font-weight:600; color:${typeColor};">
              <i class="${typeIcon}"></i> ${typeText} למורה ${teacherName}
            </div>
            <div style="font-size:11px; color:#94a3b8;">${date}</div>
          </div>
          ${tagsHtml ? `<div style="margin-bottom:4px; display:flex; flex-wrap:wrap; gap:6px;">${tagsHtml}</div>` : ''}
          ${r.text ? `<div style="font-size:13px; color:#cbd5e1; white-space:pre-wrap;">${r.text}</div>` : ''}
        </div>
      `;
    });
  },

  renderAdmin: async () => {
    const list = document.getElementById('admin-list');
    if (list) {
      list.innerHTML = '';
      app.teachers.forEach(t => {
        list.innerHTML += `
          <div class="glass" style="padding:15px; margin-bottom:10px; display:flex; justify-content:space-between; border-radius:12px; align-items:center;">
            <span>${t.name} ${t.subject ? `- ${t.subject}` : ''}</span>
            <button onclick="app.delTeacher(${t.id})" style="background:#ef4444; border:none; color:white; border-radius:8px; padding:8px 12px; cursor:pointer;">
              <i class="fas fa-trash"></i>
            </button>
          </div>`;
      });
    }

    const fbListEl = document.getElementById('admin-feedback-list');
    if (!fbListEl) return;
    fbListEl.innerHTML = '<div style="color:#94a3b8; font-size:14px;">טוען משובים...</div>';

    let { data: feed } = await supabase
      .from('feedback')
      .select('id, created_at, type, user_name, text, tags, teachers(name)')
      .order('created_at', { ascending: false })
      .limit(50);

    feed = feed || [];

    if (feed.length === 0) {
      fbListEl.innerHTML = '<div style="padding:10px; text-align:center; color:#94a3b8;">אין עדיין משובים במערכת.</div>';
      return;
    }

    fbListEl.innerHTML = '';

    feed.forEach(r => {
      const isPos = r.type === 'compliment';
      const typeColor = isPos ? 'var(--success)' : 'var(--danger)';
      const typeLabel = isPos ? 'מחמאה' : 'הערה';
      const typeIcon = isPos ? 'fas fa-heart' : 'fas fa-bolt';
      const teacherName = r.teachers ? r.teachers.name : 'מורה לא ידוע';
      const date = new Date(r.created_at).toLocaleString('he-IL', {
        day: '2-digit',
        month: '2-digit',
        year: '2-digit',
        hour: '2-digit',
        minute: '2-digit'
      });

      const tagsHtml = r.tags && r.tags.length
        ? r.tags.map(tag => `
            <span style="background:${isPos ? 'rgba(16,185,129,0.2)' : 'rgba(239,68,68,0.2)'};
                         color:${typeColor};
                         padding:3px 8px;
                         border-radius:999px;
                         font-size:11px;">
              ${tag}
            </span>`
          ).join(' ')
        : '';

      const shortText = r.text
        ? (r.text.length > 80 ? r.text.slice(0, 80) + '...' : r.text)
        : '';

      fbListEl.innerHTML += `
        <div style="display:flex; justify-content:space-between; gap:10px; padding:10px 0; border-bottom:1px solid rgba(148,163,184,0.25);">
          <div style="flex:1;">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:4px;">
              <div style="font-size:13px; color:#94a3b8;">
                <span style="color:${typeColor}; font-weight:600;">
                  <i class="${typeIcon}"></i> ${typeLabel}
                </span>
                &nbsp;|&nbsp; מורה: <strong>${teacherName}</strong>
                &nbsp;|&nbsp; תלמיד: <strong>${r.user_name || '—'}</strong>
              </div>
              <div style="font-size:11px; color:#64748b;">${date}</div>
            </div>
            ${tagsHtml ? `<div style="display:flex; flex-wrap:wrap; gap:4px; margin-bottom:4px;">${tagsHtml}</div>` : ''}
            ${shortText ? `<div style="font-size:12px; color:#cbd5e1;">${shortText}</div>` : ''}
          </div>
          <div style="display:flex; align-items:center;">
            <button onclick="app.deleteFeedback(${r.id})"
                    style="background:#ef4444; border:none; color:white; border-radius:999px; padding:6px 10px; cursor:pointer; font-size:12px; display:flex; align-items:center; gap:4px;">
              <i class="fas fa-trash"></i> מחק
            </button>
          </div>
        </div>
      `;
    });
  },
  
  addTeacher: async () => {
    const name = document.getElementById('new-t-name').value.trim();
    const sub = document.getElementById('new-t-sub').value.trim();
    if (name) {
      await supabase.from('teachers').insert([{ name, subject: sub }]);
      document.getElementById('new-t-name').value = '';
      document.getElementById('new-t-sub').value = '';
      await app.fetchData();
      app.renderAdmin();
      app.renderTeachers();
    }
  },

  delTeacher: async (id) => {
    if (confirm('למחוק מורה זה?')) {
      await supabase.from('feedback').delete().eq('teacher_id', id);
      await supabase.from('teachers').delete().eq('id', id);
      await app.fetchData();
      app.renderAdmin();
      app.renderTeachers();
    }
  },

  deleteFeedback: async (id) => {
    if (!app.user || app.user.role !== 'admin') {
      alert('רק אדמין יכול למחוק משובים');
      return;
    }

    if (!confirm('למחוק את המשוב הזה? אי אפשר לבטל.')) return;

    await supabase.from('feedback').delete().eq('id', id);

    app.renderReports();
    app.renderTeachers();
    app.updateLeaderboard();
    app.renderAdmin();
  }
};

// Login Logic
const loginBtn = document.getElementById('login-button');
if (loginBtn) {
  loginBtn.onclick = () => {
    const username = document.getElementById('login-username').value.trim();
    const password = document.getElementById('login-password').value.trim();

    if (!username) {
      alert('צריך לכתוב שם משתמש');
      return;
    }

    const isAdmin = (username === 'adir' && password === '1234');
    app.user = {
      name: username,
      role: isAdmin ? 'admin' : 'student'
    };

    localStorage.setItem('tf_user', JSON.stringify(app.user));
    app.updateUserChip();

    sfx.success();
    app.showScreen('home');
  };
}

// FX Helpers
function stringToColor(str) {
  const colors = ['#f59e0b', '#ef4444', '#10b981', '#3b82f6', '#8b5cf6', '#ec4899'];
  return colors[str.length % colors.length];
}

function flashScreen(color) {
  const f = document.getElementById('flash-fx');
  f.style.background = color;
  f.style.opacity = 0.2;
  setTimeout(() => f.style.opacity = 0, 300);
}

function triggerConfetti(icon1, icon2, icon3) {
  for(let i=0; i<30; i++) {
    const el = document.createElement('div');
    el.className = 'particle';
    el.innerText = [icon1, icon2, icon3][Math.floor(Math.random()*3)];
    el.style.left = '50%';
    el.style.top = '50%';
    const tx = (Math.random() - 0.5) * window.innerWidth;
    const ty = (Math.random() - 0.5) * window.innerHeight;
    const rot = Math.random() * 360 + 'deg';
    el.style.setProperty('--tx', `${tx}px`);
    el.style.setProperty('--ty', `${ty}px`);
    el.style.setProperty('--rot', rot);
    document.body.appendChild(el);
    setTimeout(() => el.remove(), 1000);
  }
}

document.addEventListener('DOMContentLoaded', app.init);
</script>
</body>
</html>
