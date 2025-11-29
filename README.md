<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>מערכת משוב למורים | 2.0</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    :root {
      --primary: #3b82f6;
      --primary-glow: rgba(59, 130, 246, 0.5);
      --secondary: #8b5cf6;
      --success: #10b981;
      --danger: #ef4444;
      --dark-bg: #0f172a;
      --card-bg: rgba(30, 41, 59, 0.7);
      --glass-border: rgba(255, 255, 255, 0.08);
      --text-main: #f1f5f9;
      --text-muted: #94a3b8;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      font-family: 'Rubik', sans-serif;
      background-color: var(--dark-bg);
      color: var(--text-main);
      line-height: 1.6;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* רקע אנימטיבי */
    .bg-animation {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      overflow: hidden;
      background: radial-gradient(circle at top right, #1e1b4b, #0f172a);
    }

    .blob {
      position: absolute;
      border-radius: 50%;
      filter: blur(60px);
      opacity: 0.4;
      animation: float 20s infinite ease-in-out;
    }

    .blob-1 { width: 300px; height: 300px; background: #3b82f6; top: -50px; left: -50px; animation-delay: 0s; }
    .blob-2 { width: 400px; height: 400px; background: #8b5cf6; bottom: -100px; right: -50px; animation-delay: -5s; }
    .blob-3 { width: 200px; height: 200px; background: #10b981; top: 40%; left: 40%; animation-delay: -10s; opacity: 0.2; }

    @keyframes float {
      0%, 100% { transform: translate(0, 0) scale(1); }
      33% { transform: translate(30px, -50px) scale(1.1); }
      66% { transform: translate(-20px, 20px) scale(0.9); }
    }

    /* Glassmorphism Utility */
    .glass {
      background: var(--card-bg);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid var(--glass-border);
      box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
    }

    .app {
      max-width: 800px;
      margin: 0 auto;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      position: relative;
    }

    /* Header */
    .app-header {
      padding: 16px 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 50;
      background: rgba(15, 23, 42, 0.85);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid var(--glass-border);
    }

    .logo-area {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .logo-icon {
      width: 40px;
      height: 40px;
      border-radius: 12px;
      background: linear-gradient(135deg, var(--primary), var(--secondary));
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 20px;
      box-shadow: 0 0 15px var(--primary-glow);
    }

    .app-title {
      font-weight: 700;
      font-size: 20px;
      letter-spacing: -0.5px;
    }

    .header-actions {
      display: flex;
      gap: 10px;
    }

    .icon-btn {
      width: 40px;
      height: 40px;
      border-radius: 12px;
      border: 1px solid var(--glass-border);
      background: rgba(255, 255, 255, 0.05);
      color: var(--text-main);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.2s;
      position: relative;
    }

    .icon-btn:hover {
      background: rgba(255, 255, 255, 0.1);
      transform: translateY(-2px);
    }

    .notif-badge {
      position: absolute;
      top: -2px;
      right: -2px;
      background: var(--danger);
      color: white;
      font-size: 10px;
      font-weight: bold;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 2px solid var(--dark-bg);
    }

    /* Main Content */
    .app-main {
      padding: 24px 20px;
      flex: 1;
      padding-bottom: 80px;
    }

    .screen {
      display: none;
      animation: fadeIn 0.4s ease-out;
    }

    .screen.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* Buttons */
    .btn {
      border: none;
      outline: none;
      border-radius: 14px;
      padding: 14px 24px;
      font-size: 16px;
      font-weight: 600;
      font-family: inherit;
      cursor: pointer;
      transition: transform 0.1s, box-shadow 0.2s;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }

    .btn:active { transform: scale(0.96); }

    .btn-primary {
      background: linear-gradient(135deg, var(--primary), #2563eb);
      color: white;
      box-shadow: 0 4px 15px rgba(59, 130, 246, 0.4);
    }

    .btn-success {
      background: linear-gradient(135deg, var(--success), #059669);
      color: white;
      box-shadow: 0 4px 15px rgba(16, 185, 129, 0.4);
    }

    .btn-danger {
      background: linear-gradient(135deg, var(--danger), #dc2626);
      color: white;
      box-shadow: 0 4px 15px rgba(239, 68, 68, 0.4);
    }

    .btn-full { width: 100%; }

    /* Login Screen */
    .login-card {
      max-width: 400px;
      margin: 40px auto;
      padding: 32px;
      border-radius: 24px;
      text-align: center;
    }

    .login-title { font-size: 28px; margin-bottom: 10px; font-weight: 700; }
    .input-group { margin-bottom: 20px; text-align: right; }
    .input-label { display: block; margin-bottom: 8px; font-size: 14px; color: var(--text-muted); }
    
    .styled-input {
      width: 100%;
      background: rgba(15, 23, 42, 0.5);
      border: 1px solid var(--glass-border);
      border-radius: 12px;
      padding: 14px;
      color: white;
      font-size: 16px;
      font-family: inherit;
      transition: all 0.3s;
    }
    
    .styled-input:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.2);
      outline: none;
    }

    /* Dashboard Tiles */
    .grid-menu {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
      gap: 16px;
    }

    .menu-tile {
      border-radius: 20px;
      padding: 24px;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      gap: 12px;
      cursor: pointer;
      transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
      position: relative;
      overflow: hidden;
    }

    .menu-tile::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0; bottom: 0;
      background: linear-gradient(135deg, rgba(255,255,255,0.1), transparent);
      opacity: 0;
      transition: opacity 0.3s;
    }

    .menu-tile:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.3);
    }
    .menu-tile:hover::before { opacity: 1; }

    .tile-icon {
      font-size: 32px;
      margin-bottom: 4px;
    }
    .tile-title { font-weight: 600; font-size: 16px; }
    .tile-sub { font-size: 12px; color: var(--text-muted); }

    /* Teachers List */
    .teacher-card {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 16px;
      border-radius: 16px;
      margin-bottom: 12px;
      cursor: pointer;
      transition: transform 0.2s;
    }
    
    .teacher-card:hover { background: rgba(255, 255, 255, 0.08); }

    .teacher-info { display: flex; align-items: center; gap: 16px; }
    
    .avatar {
      width: 48px;
      height: 48px;
      border-radius: 14px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 20px;
      font-weight: 700;
      color: white;
      text-shadow: 0 2px 4px rgba(0,0,0,0.2);
    }

    /* Stats & Profile */
    .profile-stats {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      gap: 12px;
      margin: 20px 0;
    }

    .stat-box {
      background: rgba(255,255,255,0.03);
      padding: 16px;
      border-radius: 16px;
      text-align: center;
      border: 1px solid var(--glass-border);
    }

    .stat-val { font-size: 24px; font-weight: 700; display: block; }
    .stat-lbl { font-size: 12px; color: var(--text-muted); }

    .text-green { color: var(--success); }
    .text-red { color: var(--danger); }
    .text-blue { color: var(--primary); }

    /* Feedback Form */
    .tags-container {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin: 16px 0;
    }

    .tag-btn {
      background: rgba(255,255,255,0.05);
      border: 1px solid var(--glass-border);
      color: var(--text-muted);
      padding: 8px 16px;
      border-radius: 20px;
      font-size: 14px;
      cursor: pointer;
      transition: all 0.2s;
    }

    .tag-btn.selected {
      background: var(--primary);
      color: white;
      border-color: var(--primary);
      box-shadow: 0 0 15px rgba(59, 130, 246, 0.4);
    }
    
    .tag-btn.selected-neg {
      background: var(--danger);
      color: white;
      border-color: var(--danger);
      box-shadow: 0 0 15px rgba(239, 68, 68, 0.4);
    }

    .big-textarea {
      width: 100%;
      min-height: 120px;
      background: rgba(15, 23, 42, 0.5);
      border: 1px solid var(--glass-border);
      border-radius: 16px;
      padding: 16px;
      color: white;
      font-family: inherit;
      font-size: 16px;
      resize: vertical;
      margin-bottom: 20px;
    }

    /* Toast Notification */
    .toast {
      position: fixed;
      bottom: 30px;
      left: 50%;
      transform: translateX(-50%) translateY(100px);
      background: rgba(30, 41, 59, 0.95);
      border: 1px solid var(--glass-border);
      padding: 16px 24px;
      border-radius: 50px;
      display: flex;
      align-items: center;
      gap: 12px;
      box-shadow: 0 20px 40px rgba(0,0,0,0.5);
      z-index: 1000;
      opacity: 0;
      transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }

    .toast.show {
      transform: translateX(-50%) translateY(0);
      opacity: 1;
    }

    .toast i { font-size: 20px; }

    /* Leaderboard */
    .leaderboard-row {
      display: flex;
      align-items: center;
      padding: 12px;
      border-bottom: 1px solid rgba(255,255,255,0.05);
    }
    .rank-num {
      width: 30px;
      font-weight: 700;
      color: var(--text-muted);
    }
    .rank-1 { color: #ffd700; text-shadow: 0 0 10px rgba(255, 215, 0, 0.5); }
    .rank-2 { color: #c0c0c0; }
    .rank-3 { color: #cd7f32; }

    /* Particles / Confetti */
    .particle {
      position: absolute;
      pointer-events: none;
      animation: pop 0.6s ease-out forwards;
    }

    @keyframes pop {
      0% { transform: translate(0, 0) scale(1); opacity: 1; }
      100% { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; }
    }

    /* Utility */
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }
    .back-btn {
      background: none;
      border: none;
      color: var(--text-muted);
      cursor: pointer;
      font-size: 16px;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .back-btn:hover { color: white; }

  </style>
</head>
<body>

  <div class="bg-animation">
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
  </div>

  <div class="app">
    <header class="app-header">
      <div class="logo-area">
        <div class="logo-icon"><i class="fas fa-graduation-cap"></i></div>
        <div class="app-title">TeacherFeed</div>
      </div>
      <div class="header-actions">
        <button class="icon-btn" id="notif-btn" onclick="soundManager.play('click')">
          <i class="fas fa-bell"></i>
          <span class="notif-badge" id="notif-badge" style="display:none">0</span>
        </button>
        <button class="icon-btn" id="logout-btn" style="display:none" onclick="app.logout()">
          <i class="fas fa-sign-out-alt"></i>
        </button>
      </div>
    </header>

    <main class="app-main">
      
      <section id="screen-login" class="screen active">
        <div class="login-card glass">
          <div class="login-title">ברוכים הבאים</div>
          <div style="color:var(--text-muted); margin-bottom:24px;">המקום שלך להשפיע על הלמידה</div>
          
          <div class="input-group">
            <label class="input-label">שם משתמש</label>
            <input type="text" id="login-username" class="styled-input" placeholder="לדוגמה: דניאל">
          </div>
          
          <div class="input-group">
            <label class="input-label">סיסמה (אופציונלי)</label>
            <input type="password" id="login-password" class="styled-input" placeholder="••••••">
          </div>

          <button class="btn btn-primary btn-full" onclick="app.login()">
            התחברות למערכת <i class="fas fa-arrow-left"></i>
          </button>
        </div>
      </section>

      <section id="screen-home" class="screen">
        <h2 style="margin-bottom: 8px;">שלום, <span id="user-greeting" class="text-blue"></span> 👋</h2>
        <p style="color:var(--text-muted); margin-bottom: 24px;">מה תרצה לעשות היום?</p>

        <div class="grid-menu">
          <div class="menu-tile glass" onclick="app.navTo('teachers', 'compliment')">
            <div class="tile-icon text-green"><i class="fas fa-heart"></i></div>
            <div class="tile-title">לפרגן למורה</div>
            <div class="tile-sub">מגיע להם מילה טובה</div>
          </div>

          <div class="menu-tile glass" onclick="app.navTo('teachers', 'remark')">
            <div class="tile-icon text-red"><i class="fas fa-comment-slash"></i></div>
            <div class="tile-title">להעיר למורה</div>
            <div class="tile-sub">חשוב שידעו מה לשפר</div>
          </div>

          <div class="menu-tile glass" onclick="app.navTo('leaderboard')">
            <div class="tile-icon text-blue"><i class="fas fa-trophy"></i></div>
            <div class="tile-title">טבלת מובילים</div>
            <div class="tile-sub">מי התלמיד הכי משפיע?</div>
          </div>

          <div class="menu-tile glass" onclick="app.navTo('reports')">
            <div class="tile-icon" style="color:#f59e0b"><i class="fas fa-chart-pie"></i></div>
            <div class="tile-title">הדוחות שלי</div>
            <div class="tile-sub">היסטוריית הפעילות שלך</div>
          </div>

           <div class="menu-tile glass" id="admin-tile" style="display:none" onclick="app.navTo('admin')">
            <div class="tile-icon" style="color:#8b5cf6"><i class="fas fa-shield-alt"></i></div>
            <div class="tile-title">ניהול (Admin)</div>
            <div class="tile-sub">הוספת מורים ונתונים</div>
          </div>
        </div>
      </section>

      <section id="screen-teachers" class="screen">
        <div class="section-header">
          <h2>בחירת מורה</h2>
          <button class="back-btn" onclick="app.navTo('home')"><i class="fas fa-arrow-right"></i> חזרה</button>
        </div>
        <input type="text" id="search-teacher" class="styled-input" placeholder="חיפוש מורה..." style="margin-bottom:20px;" oninput="app.renderTeacherList()">
        <div id="teachers-container"></div>
      </section>

      <section id="screen-feedback" class="screen">
        <div class="section-header">
          <button class="back-btn" onclick="app.navTo('teachers')"><i class="fas fa-arrow-right"></i> ביטול</button>
        </div>

        <div class="glass" style="padding:24px; border-radius:24px; text-align:center;">
          <div class="avatar" id="feedback-avatar" style="width:80px; height:80px; font-size:32px; margin:0 auto 16px;"></div>
          <h2 id="feedback-teacher-name">שם המורה</h2>
          <div id="feedback-subject" style="color:var(--text-muted); margin-bottom:20px;">מקצוע</div>

          <div class="profile-stats">
            <div class="stat-box">
              <span class="stat-val text-green" id="stat-comp">0</span>
              <span class="stat-lbl">מחמאות</span>
            </div>
            <div class="stat-box">
              <span class="stat-val text-red" id="stat-rem">0</span>
              <span class="stat-lbl">הערות</span>
            </div>
            <div class="stat-box">
              <span class="stat-val text-blue" id="stat-score">0</span>
              <span class="stat-lbl">ציון</span>
            </div>
          </div>

          <hr style="border:0; border-top:1px solid rgba(255,255,255,0.1); margin:20px 0;">

          <h3 style="text-align:right; margin-bottom:12px;">מה תרצה להגיד?</h3>
          <div id="quick-tags" class="tags-container"></div>
          
          <textarea id="feedback-text" class="big-textarea" placeholder="כתוב כאן בפירוט (אופציונלי)..."></textarea>

          <button id="btn-submit" class="btn btn-full btn-primary" onclick="app.submitFeedback()">
            שלח משוב
          </button>
        </div>
      </section>

      <section id="screen-leaderboard" class="screen">
        <div class="section-header">
          <h2>המשפיענים</h2>
          <button class="back-btn" onclick="app.navTo('home')"><i class="fas fa-arrow-right"></i> חזרה</button>
        </div>
        <div class="glass" style="border-radius:20px; overflow:hidden;">
          <div id="leaderboard-list"></div>
        </div>
      </section>

      <section id="screen-admin" class="screen">
        <div class="section-header">
          <h2>ניהול מערכת</h2>
          <button class="back-btn" onclick="app.navTo('home')"><i class="fas fa-arrow-right"></i> חזרה</button>
        </div>
        
        <div class="glass" style="padding:20px; border-radius:20px; margin-bottom:20px;">
          <h3>הוספת מורה חדש</h3>
          <div class="input-group" style="margin-top:10px;">
            <input type="text" id="new-teacher-name" class="styled-input" placeholder="שם המורה">
          </div>
          <div class="input-group">
            <input type="text" id="new-teacher-subject" class="styled-input" placeholder="מקצוע">
          </div>
          <button class="btn btn-success btn-full" onclick="app.addTeacher()">הוסף למערכת</button>
        </div>

        <div class="glass" style="padding:20px; border-radius:20px;">
          <h3>רשימת מורים</h3>
          <div id="admin-list" style="margin-top:10px;"></div>
        </div>
      </section>

    </main>
  </div>

  <div id="toast" class="toast">
    <div id="toast-icon"></div>
    <div id="toast-msg"></div>
  </div>

<script>
// --- Sound Engine (Web Audio API) ---
// מייצר סאונד ללא צורך בקבצים חיצוניים
const soundManager = {
  ctx: null,
  init: function() {
    window.AudioContext = window.AudioContext || window.webkitAudioContext;
    this.ctx = new AudioContext();
  },
  play: function(type) {
    if (!this.ctx) this.init();
    const t = this.ctx.currentTime;
    const osc = this.ctx.createOscillator();
    const gain = this.ctx.createGain();
    osc.connect(gain);
    gain.connect(this.ctx.destination);

    if (type === 'click') {
      osc.type = 'sine';
      osc.frequency.setValueAtTime(600, t);
      osc.frequency.exponentialRampToValueAtTime(300, t + 0.1);
      gain.gain.setValueAtTime(0.1, t);
      gain.gain.exponentialRampToValueAtTime(0.01, t + 0.1);
      osc.start(t);
      osc.stop(t + 0.1);
    } 
    else if (type === 'success') {
      osc.type = 'triangle';
      osc.frequency.setValueAtTime(440, t);
      osc.frequency.setValueAtTime(554, t + 0.1); // C#
      gain.gain.setValueAtTime(0.1, t);
      gain.gain.linearRampToValueAtTime(0, t + 0.4);
      osc.start(t);
      osc.stop(t + 0.4);
    }
    else if (type === 'error') {
      osc.type = 'sawtooth';
      osc.frequency.setValueAtTime(150, t);
      osc.frequency.linearRampToValueAtTime(100, t + 0.3);
      gain.gain.setValueAtTime(0.1, t);
      gain.gain.linearRampToValueAtTime(0, t + 0.3);
      osc.start(t);
      osc.stop(t + 0.3);
    }
    else if (type === 'pop') {
      osc.type = 'sine';
      osc.frequency.setValueAtTime(800, t);
      gain.gain.setValueAtTime(0.05, t);
      gain.gain.exponentialRampToValueAtTime(0.01, t + 0.1);
      osc.start(t);
      osc.stop(t + 0.1);
    }
  }
};

// --- App Logic ---
const SUPABASE_URL = 'https://xidfthnboggokcsloglt.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhpZGZ0aG5ib2dnb2tjc2xvZ2x0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjQzMTIwNzUsImV4cCI6MjA3OTg4ODA3NX0.fEPS2FJYlcZ4DOv7I0RBEcwZBfT0MdRGslk9cp_2GwU';
const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#ec4899', '#06b6d4'];

const app = {
  user: null,
  teachers: [],
  feedback: [],
  currentMode: null, // 'compliment' or 'remark'
  selectedTeacher: null,

  init: async () => {
    // Check local storage for user
    const savedUser = localStorage.getItem('tf_user');
    if (savedUser) {
      app.user = JSON.parse(savedUser);
      app.postLoginSetup();
    }
    await app.fetchData();
    app.setupRealtime();
  },

  fetchData: async () => {
    let { data: t } = await supabase.from('teachers').select('*');
    app.teachers = t || [];
    let { data: f } = await supabase.from('feedback').select('*');
    app.feedback = f || [];
  },

  setupRealtime: () => {
    supabase.channel('public:feedback').on('postgres_changes', { event: '*', schema: 'public', table: 'feedback' }, payload => {
      app.fetchData().then(() => {
        if (app.user) app.updateLeaderboard(); // Refresh if viewing leaderboard
      });
    }).subscribe();
  },

  login: () => {
    const name = document.getElementById('login-username').value.trim();
    const pass = document.getElementById('login-password').value.trim();
    if (!name) return app.toast('נא להזין שם משתמש', 'error');

    soundManager.play('success');
    app.user = { 
      name: name, 
      role: (name === 'adir' && pass === '1234') ? 'admin' : 'student' 
    };
    
    localStorage.setItem('tf_user', JSON.stringify(app.user));
    app.postLoginSetup();
  },

  postLoginSetup: () => {
    document.getElementById('screen-login').classList.remove('active');
    document.getElementById('screen-home').classList.add('active');
    document.getElementById('logout-btn').style.display = 'flex';
    document.getElementById('user-greeting').innerText = app.user.name;
    if (app.user.role === 'admin') document.getElementById('admin-tile').style.display = 'flex';
  },

  logout: () => {
    localStorage.removeItem('tf_user');
    location.reload();
  },

  navTo: (screenId, mode = null) => {
    soundManager.play('click');
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById('screen-' + screenId).classList.add('active');
    
    if (screenId === 'teachers') {
      app.currentMode = mode; // Store mode
      app.renderTeacherList();
    }
    if (screenId === 'leaderboard') app.renderLeaderboard();
    if (screenId === 'admin') app.renderAdmin();
  },

  renderTeacherList: () => {
    const container = document.getElementById('teachers-container');
    const search = document.getElementById('search-teacher').value.toLowerCase();
    container.innerHTML = '';

    app.teachers.filter(t => t.name.toLowerCase().includes(search)).forEach(t => {
      const stats = app.getStats(t.id);
      const color = colors[t.name.length % colors.length];
      
      const el = document.createElement('div');
      el.className = 'teacher-card glass';
      el.innerHTML = `
        <div class="teacher-info">
          <div class="avatar" style="background:${color}">${t.name[0]}</div>
          <div>
            <div style="font-weight:600; font-size:16px;">${t.name}</div>
            <div style="font-size:13px; color:var(--text-muted)">${t.subject}</div>
          </div>
        </div>
        <div style="text-align:left">
          <div style="font-weight:700; color:${stats.score >= 0 ? 'var(--success)' : 'var(--danger)'}">${stats.score > 0 ? '+' : ''}${stats.score}</div>
          <div style="font-size:10px; color:var(--text-muted)">נקודות</div>
        </div>
      `;
      el.onclick = () => app.openFeedback(t);
      container.appendChild(el);
    });
  },

  openFeedback: (teacher) => {
    // If no mode selected (came from admin or somewhere else), default to profile view
    if (!app.currentMode) app.currentMode = 'compliment'; 

    app.selectedTeacher = teacher;
    soundManager.play('pop');
    
    // UI Update
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById('screen-feedback').classList.add('active');
    
    document.getElementById('feedback-teacher-name').innerText = teacher.name;
    document.getElementById('feedback-subject').innerText = teacher.subject;
    
    const color = colors[teacher.name.length % colors.length];
    const avatar = document.getElementById('feedback-avatar');
    avatar.style.background = color;
    avatar.innerText = teacher.name[0];

    // Stats
    const stats = app.getStats(teacher.id);
    document.getElementById('stat-comp').innerText = stats.comp;
    document.getElementById('stat-rem').innerText = stats.rem;
    document.getElementById('stat-score').innerText = stats.score;

    // Tags Setup
    const tagsContainer = document.getElementById('quick-tags');
    tagsContainer.innerHTML = '';
    const tags = app.currentMode === 'compliment' 
      ? ['הסבר ברור', 'יחס מעולה', 'שיעור מעניין', 'עוזר לתלמידים', 'סבלני']
      : ['הסבר לא מובן', 'יחס לא מכבד', 'איחור לשיעור', 'חוסר סבלנות', 'ציון לא הוגן'];
    
    tags.forEach(tag => {
      const btn = document.createElement('div');
      btn.className = 'tag-btn';
      btn.innerText = tag;
      btn.onclick = function() {
        soundManager.play('click');
        this.classList.toggle(app.currentMode === 'compliment' ? 'selected' : 'selected-neg');
      };
      tagsContainer.appendChild(btn);
    });

    // Button Style
    const btn = document.getElementById('btn-submit');
    if (app.currentMode === 'compliment') {
      btn.className = 'btn btn-full btn-success';
      btn.innerHTML = '<i class="fas fa-heart"></i> שלח פירגון';
    } else {
      btn.className = 'btn btn-full btn-danger';
      btn.innerHTML = '<i class="fas fa-exclamation-triangle"></i> שלח הערה';
    }
  },

  submitFeedback: async () => {
    const text = document.getElementById('feedback-text').value;
    const activeTags = Array.from(document.querySelectorAll('.tag-btn.selected, .tag-btn.selected-neg')).map(e => e.innerText);

    if (!text && activeTags.length === 0) return app.toast('צריך לכתוב משהו או לבחור תגיות', 'error');

    const feedbackData = {
      teacher_id: app.selectedTeacher.id,
      user_name: app.user.name,
      type: app.currentMode,
      tags: activeTags,
      text: text
    };

    // Optimistic UI
    app.confetti();
    soundManager.play('success');
    app.toast(app.currentMode === 'compliment' ? 'הפרגון נשלח!' : 'ההערה נרשמה', 'success');
    app.navTo('teachers', app.currentMode);

    // Send to DB
    await supabase.from('feedback').insert([feedbackData]);
    document.getElementById('feedback-text').value = '';
  },

  getStats: (teacherId) => {
    const f = app.feedback.filter(x => x.teacher_id === teacherId);
    const comp = f.filter(x => x.type === 'compliment').length;
    const rem = f.filter(x => x.type === 'remark').length;
    return { comp, rem, score: comp - rem };
  },

  renderLeaderboard: () => {
    const list = document.getElementById('leaderboard-list');
    list.innerHTML = '';
    
    // Calculate top students
    const studentCounts = {};
    app.feedback.forEach(f => {
      if(!studentCounts[f.user_name]) studentCounts[f.user_name] = 0;
      studentCounts[f.user_name]++;
    });

    const sorted = Object.entries(studentCounts).sort((a,b) => b[1] - a[1]);

    if (sorted.length === 0) list.innerHTML = '<div style="padding:20px; text-align:center">אין עדיין נתונים</div>';

    sorted.forEach((item, index) => {
      const rankClass = index === 0 ? 'rank-1' : (index === 1 ? 'rank-2' : (index === 2 ? 'rank-3' : ''));
      const icon = index === 0 ? '<i class="fas fa-crown"></i>' : index + 1;
      
      const row = document.createElement('div');
      row.className = 'leaderboard-row';
      row.innerHTML = `
        <div class="rank-num ${rankClass}">${icon}</div>
        <div style="flex:1; font-weight:500;">${item[0]}</div>
        <div class="tag-btn" style="border-radius:8px; cursor:default">${item[1]} משובים</div>
      `;
      list.appendChild(row);
    });
  },

  renderAdmin: () => {
    const list = document.getElementById('admin-list');
    list.innerHTML = '';
    app.teachers.forEach(t => {
      const d = document.createElement('div');
      d.className = 'teacher-card glass';
      d.style.marginBottom = '8px';
      d.innerHTML = `
        <span>${t.name} (${t.subject})</span>
        <button class="btn btn-danger" style="padding:6px 12px; font-size:12px;" onclick="app.deleteTeacher(${t.id})"><i class="fas fa-trash"></i></button>
      `;
      list.appendChild(d);
    });
  },

  addTeacher: async () => {
    const name = document.getElementById('new-teacher-name').value;
    const subject = document.getElementById('new-teacher-subject').value;
    if(name && subject) {
      await supabase.from('teachers').insert([{name, subject}]);
      soundManager.play('success');
      app.renderAdmin();
      document.getElementById('new-teacher-name').value = '';
      document.getElementById('new-teacher-subject').value = '';
    }
  },

  deleteTeacher: async (id) => {
    if(confirm('למחוק? זה ימחק גם את המשובים שלו.')) {
      await supabase.from('feedback').delete().eq('teacher_id', id);
      await supabase.from('teachers').delete().eq('id', id);
      app.renderAdmin();
    }
  },

  toast: (msg, type) => {
    const t = document.getElementById('toast');
    const icon = document.getElementById('toast-icon');
    document.getElementById('toast-msg').innerText = msg;
    
    t.style.borderLeft = type === 'success' ? '4px solid var(--success)' : '4px solid var(--danger)';
    icon.innerHTML = type === 'success' ? '<i class="fas fa-check-circle text-green"></i>' : '<i class="fas fa-exclamation-circle text-red"></i>';
    
    soundManager.play(type === 'success' ? 'success' : 'error');
    t.classList.add('show');
    setTimeout(() => t.classList.remove('show'), 3000);
  },

  confetti: () => {
    const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444'];
    for (let i = 0; i < 30; i++) {
      const p = document.createElement('div');
      p.className = 'particle';
      p.style.left = '50%';
      p.style.top = '50%';
      p.style.width = Math.random() * 8 + 4 + 'px';
      p.style.height = p.style.width;
      p.style.background = colors[Math.floor(Math.random() * colors.length)];
      p.style.borderRadius = '50%';
      
      const tx = (Math.random() - 0.5) * 300 + 'px';
      const ty = (Math.random() - 0.5) * 300 + 'px';
      p.style.setProperty('--tx', tx);
      p.style.setProperty('--ty', ty);
      
      document.body.appendChild(p);
      setTimeout(() => p.remove(), 600);
    }
  }
};

// Start
document.addEventListener('DOMContentLoaded', app.init);
</script>
</body>
</html>
