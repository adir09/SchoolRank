<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>מערכת משוב למורים</title>
  <meta name="description" content="מערכת משוב דיגיטלית למורים - אפליקציה לניהול משובים על מורים">
  <meta name="keywords" content="מורים, משוב, חינוך, ישראל, תלמידים">
  <meta name="author" content="שם שלך">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    /* כל ה-CSS הקיים נשאר ללא שינוי */
    /* ... */
  </style>
</head>
<body>
<div class="app">
  <!-- Notifications panel -->
  <div id="notif-panel" class="notif-panel" aria-hidden="true">
    <div class="notif-header">
      <span>התראות</span>
      <span style="font-size:12px;color:#94a3b8;">סיכום חכם לפי המשובים שלך</span>
    </div>
    <div class="notif-sub">אין פה ספאם, רק תובנות קצרות.</div>
    <div id="notif-list" class="notif-list"></div>
  </div>

  <!-- Top Bar -->
  <header class="app-header">
    <div class="app-header-left">
      <div class="logo"><i class="fas fa-chalkboard-teacher"></i></div>
      <div class="app-title">מערכת משוב למורים</div>
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

  <!-- Main -->
  <main class="app-main">
    <!-- Screen: Login -->
    <section id="screen-login" class="screen active">
      <div class="login-wrapper">
        <div class="login-title">התחברות למערכת</div>
        <div class="login-sub">
          אתה יכול לכתוב כל שם משתמש וכל סיסמה – המערכת תשמור אותם במחשב שלך.<br>
          לדוגמה: <b>תלמיד</b> או כל שם אחר שבא לך.<br>
          <b>המערכת תזכור אותך ותכניס אותך אוטומטית בפעם הבאה!</b>
        </div>

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

        <button class="btn btn-full btn-green" id="login-button">
          <i class="fas fa-sign-in-alt"></i>
          <span>התחברות</span>
        </button>
        
        <div style="margin-top: 16px; padding: 12px; background: rgba(59, 130, 246, 0.1); border-radius: 8px; border: 1px solid rgba(59, 130, 246, 0.3);">
          <div style="font-size: 12px; color: #93c5fd; display: flex; align-items: center; gap: 6px;">
            <i class="fas fa-cloud"></i>
            <span>המערכת מחוברת ל-Supabase - כל המשובים נשמרים ונראים לכל המשתמשים!</span>
          </div>
          <div id="sync-status" class="sync-status sync-online">
            <i class="fas fa-database"></i>
            <span>מחובר - נתונים משותפים</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Screen: Home -->
    <section id="screen-home" class="screen">
      <div class="greeting" id="home-greeting">מה אתה רוצה לעשות היום?</div>
      <div class="sub-greeting" id="home-sub">מערכת משוב. לא מושלמת, אבל לפחות היא בצד שלך.</div>

      <div class="tiles-grid" id="home-tiles">
        <div class="tile tile-primary" data-action="view-teachers">
          <div class="tile-header">
            <div>
              <div class="tile-title">צפייה במורים</div>
              <div class="tile-desc">לעבור על כל המורים, לראות מי זוכה בנקודות.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-users"></i></div>
          </div>
        </div>

        <div class="tile tile-primary" data-action="quick-compliment">
          <div class="tile-header">
            <div>
              <div class="tile-title">הוספת מחמאה</div>
              <div class="tile-desc">הסביר ברור, היה נחמד, היה אנושי – מגיע לו.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-heart"></i></div>
          </div>
        </div>

        <div class="tile tile-danger" data-action="quick-remark">
          <div class="tile-header">
            <div>
              <div class="tile-title">הוספת הערה</div>
              <div class="tile-desc">לא ברור, לא הוגן, או פשוט לא. כותבים את זה.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-exclamation-triangle"></i></div>
          </div>
        </div>

        <div class="tile tile-neutral" data-action="view-reports">
          <div class="tile-header">
            <div>
              <div class="tile-title">הדוחות שלי</div>
              <div class="tile-desc">כל מה שכבר כתבת – טוב ורע – במקום אחד.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-chart-bar"></i></div>
          </div>
        </div>

        <div class="tile tile-neutral" data-action="leaderboard">
          <div class="tile-header">
            <div>
              <div class="tile-title">לוח מדרגים</div>
              <div class="tile-desc">מי הכי פעיל במתן משובים.</div>
            </div>
            <div class="tile-icon"><i class="fas fa-trophy"></i></div>
          </div>
        </div>

        <div class="tile tile-admin" data-action="admin" id="admin-tile" style="display:none;">
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

    <!-- Screen: Teacher List -->
    <section id="screen-teachers" class="screen">
      <div class="section-header">
        <div class="section-title">רשימת מורים</div>
        <div class="back-link" data-back-to="home">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
      </div>
      <div class="search-bar">
        <input id="teacher-search" class="search-input" type="text" placeholder="חיפוש לפי שם או מקצוע...">
        <div class="search-icon"><i class="fas fa-search"></i></div>
      </div>
      <div class="teacher-list" id="teacher-list"></div>
    </section>

    <!-- Screen: Teacher Profile -->
    <section id="screen-teacher-profile" class="screen">
      <div class="section-header">
        <div class="section-title" id="teacher-profile-name">שם מורה</div>
        <div class="back-link" data-back-to="teachers">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
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
        <button class="btn btn-green" id="btn-profile-compliment">
          <i class="fas fa-heart"></i>
          <span>הוסף מחמאה</span>
        </button>
        <button class="btn btn-red" id="btn-profile-remark">
          <i class="fas fa-exclamation-triangle"></i>
          <span>הוסף הערה</span>
        </button>
      </div>

      <h3 style="font-size:18px;margin:20px 0 12px;">
        <i class="fas fa-history"></i>
        <span>משובים אחרונים</span>
      </h3>
      <div id="teacher-feedback-list" class="feedback-list"></div>
      <div id="teacher-feedback-empty" class="feedback-empty">
        <i class="fas fa-inbox"></i>
        <div>אין עדיין משוב.</div>
      </div>
    </section>

    <!-- Screen: Feedback Form -->
    <section id="screen-feedback" class="screen">
      <div class="section-header">
        <div class="section-title">הוספת משוב</div>
        <div class="back-link" id="feedback-back">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
      </div>

      <div class="feedback-header">
        <div class="feedback-header-title" id="feedback-title"></div>
        <div class="feedback-header-sub" id="feedback-subtitle"></div>
      </div>

      <div class="quick-tags" id="quick-tags"></div>

      <textarea id="feedback-text" class="textarea" placeholder="תכתוב מה שחשוב לך."></textarea>

      <button class="btn btn-full" id="feedback-submit-button">
        <i class="fas fa-paper-plane"></i>
        <span>שליחת משוב</span>
      </button>
    </section>

    <!-- Screen: Reports -->
    <section id="screen-reports" class="screen">
      <div class="section-header">
        <div class="section-title">הדוחות שלי</div>
        <div class="back-link" data-back-to="home">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
      </div>

      <div class="reports-section">
        <div class="report-summary-card">
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

        <div class="report-summary-card">
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

        <div class="report-summary-card">
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
    </section>

    <!-- Screen: Leaderboard -->
    <section id="screen-leaderboard" class="screen">
      <div class="section-header">
        <div class="section-title">לוח מדרגים</div>
        <div class="back-link" data-back-to="home">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
      </div>

      <div class="chip">
        <i class="fas fa-trophy"></i>
        <span>מי נותן הכי הרבה משובים</span>
      </div>

      <div class="reports-section" style="margin-top:20px;">
        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
            <i class="fas fa-users"></i>
            <strong>טבלת תלמידים</strong>
          </div>
          <div>
            <table class="leaderboard-table">
              <thead>
                <tr>
                  <th>#</th>
                  <th>שם משתמש</th>
                  <th>מחמאות</th>
                  <th>הערות</th>
                  <th>סה״כ</th>
                  <th>פעילות</th>
                </tr>
              </thead>
              <tbody id="leaderboard-body"></tbody>
            </table>
          </div>
        </div>
      </div>
    </section>

    <!-- Screen: Admin -->
    <section id="screen-admin" class="screen">
      <div class="section-header">
        <div class="section-title">ניהול מורים</div>
        <div class="back-link" data-back-to="home">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
      </div>

      <div class="chip">
        <i class="fas fa-shield-alt"></i>
        <span>מצב אדמין – רק אתה רואה את זה</span>
      </div>

      <div class="admin-grid">
        <div class="admin-panel">
          <div class="admin-panel-title">
            <i class="fas fa-user-plus"></i>
            <span>הוספת מורה חדש</span>
          </div>
          <div class="admin-form-row">
            <input type="text" id="admin-new-name" class="text-input" placeholder="שם המורה">
            <input type="text" id="admin-new-subject" class="text-input" placeholder="מקצוע">
          </div>
          <button class="btn btn-green btn-full" id="admin-add-teacher">
            <i class="fas fa-plus"></i>
            <span>הוסף מורה</span>
          </button>
          <div class="admin-note">
            <i class="fas fa-info-circle"></i>
            <span>הנתונים נשמרים ב-Supabase ונראים לכל המשתמשים.</span>
          </div>
        </div>

        <div class="admin-panel">
          <div class="admin-panel-title">
            <i class="fas fa-list"></i>
            <span>רשימת מורים (אדמין)</span>
          </div>
          <div id="admin-teacher-list" class="teacher-list"></div>
        </div>
      </div>
    </section>

  </main>
</div>

<script>
  // ---------- הגדרות Supabase ----------
  const SUPABASE_URL = 'https://xidfthnboggokcsloglt.supabase.co'; // החלף ב-URL האמיתי שלך
  const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhpZGZ0aG5ib2dnb2tjc2xvZ2x0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjQzMTIwNzUsImV4cCI6MjA3OTg4ODA3NX0.fEPS2FJYlcZ4DOv7I0RBEcwZBfT0MdRGslk9cp_2GwU'; // החלף ב-anon key האמיתי שלך

  // אתחול Supabase
  const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

  // ---------- נתונים גלובליים ----------
  let teachers = [];
  let feedbackEntries = [];
  let studentStats = {};

  const quickTagSets = {
    compliment: ["מסביר ברור", "יחס טוב", "שומר על סדר", "נותן משוב מועיל"],
    remark: ["הסבר לא ברור", "ציון לא הוגן", "דיבור לא מכבד", "מאחר לשיעור"]
  };

  // ---------- State ----------
  const appState = {
    currentScreen: "login",
    currentUser: null,
    selectedTeacherId: null,
    feedbackType: null
  };

  // ---------- פונקציות Cookies ----------
  function setCookie(name, value, days) {
    const expires = new Date();
    expires.setTime(expires.getTime() + (days * 24 * 60 * 60 * 1000));
    document.cookie = `${name}=${value};expires=${expires.toUTCString()};path=/`;
  }

  function getCookie(name) {
    const nameEQ = name + "=";
    const ca = document.cookie.split(';');
    for(let i = 0; i < ca.length; i++) {
      let c = ca[i];
      while (c.charAt(0) === ' ') c = c.substring(1, c.length);
      if (c.indexOf(nameEQ) === 0) return c.substring(nameEQ.length, c.length);
    }
    return null;
  }

  function deleteCookie(name) {
    document.cookie = name + '=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;';
  }

  // ---------- פונקציות שמירה וטעינה ----------
  async function loadData() {
    try {
      // טען מורים
      const { data: teachersData, error: teachersError } = await supabase
        .from('teachers')
        .select('*')
        .order('id');
      
      if (teachersError) throw teachersError;
      teachers = teachersData || [];

      // טען משובים
      const { data: feedbackData, error: feedbackError } = await supabase
        .from('feedback')
        .select('*')
        .order('created_at', { ascending: false });
      
      if (feedbackError) throw feedbackError;
      feedbackEntries = feedbackData || [];

      // טען סטטיסטיקות
      const { data: statsData, error: statsError } = await supabase
        .from('student_stats')
        .select('*');
      
      if (statsError) throw statsError;
      
      // המר את הסטטיסטיקות לאובייקט
      studentStats = {};
      if (statsData) {
        statsData.forEach(stat => {
          studentStats[stat.user_name] = {
            compliments: stat.compliments,
            remarks: stat.remarks
          };
        });
      }

      updateSyncStatus(true);
      console.log('✅ נתונים נטענו בהצלחה');

    } catch (error) {
      console.error('❌ שגיאה בטעינת נתונים:', error);
      // נתוני ברירת מחדל במקרה של שגיאה
      teachers = [
        { id: 1, name: "אורן", subject: "אנגלית" },
        { id: 2, name: "אורית", subject: "מתמטיקה" },
        { id: 3, name: "רעות", subject: "לשון" },
        { id: 4, name: "אבי", subject: "השכלה כללית" },
        { id: 5, name: "נטע", subject: "היסטוריה" }
      ];
      feedbackEntries = [];
      studentStats = {};
      updateSyncStatus(false);
    }
  }

  async function saveFeedback(feedback) {
    try {
      const { data, error } = await supabase
        .from('feedback')
        .insert([
          {
            teacher_id: feedback.teacherId,
            type: feedback.type,
            tags: feedback.tags,
            text: feedback.text,
            user_name: feedback.user
          }
        ])
        .select();

      if (error) throw error;

      // עדכן סטטיסטיקות
      await updateStudentStats(feedback.user, feedback.type);
      
      return true;
    } catch (error) {
      console.error('❌ שגיאה בשמירת משוב:', error);
      return false;
    }
  }

  async function updateStudentStats(userName, type) {
    try {
      // בדוק אם המשתמש כבר קיים
      const { data: existingStat, error: checkError } = await supabase
        .from('student_stats')
        .select('*')
        .eq('user_name', userName)
        .single();

      if (checkError && checkError.code !== 'PGRST116') throw checkError;

      if (existingStat) {
        // עדכן סטטיסטיקה קיימת
        const updateData = type === 'compliment' 
          ? { compliments: existingStat.compliments + 1 }
          : { remarks: existingStat.remarks + 1 };

        const { error: updateError } = await supabase
          .from('student_stats')
          .update(updateData)
          .eq('user_name', userName);

        if (updateError) throw updateError;
      } else {
        // צור סטטיסטיקה חדשה
        const newStat = {
          user_name: userName,
          compliments: type === 'compliment' ? 1 : 0,
          remarks: type === 'remark' ? 1 : 0
        };

        const { error: insertError } = await supabase
          .from('student_stats')
          .insert([newStat]);

        if (insertError) throw insertError;
      }

      return true;
    } catch (error) {
      console.error('❌ שגיאה בעדכון סטטיסטיקות:', error);
      return false;
    }
  }

  async function addTeacher(name, subject) {
    try {
      const { data, error } = await supabase
        .from('teachers')
        .insert([{ name, subject }])
        .select();

      if (error) throw error;
      return data[0];
    } catch (error) {
      console.error('❌ שגיאה בהוספת מורה:', error);
      return null;
    }
  }

  async function deleteTeacher(teacherId) {
    try {
      // מחק תחילה את כל המשובים של המורה
      const { error: feedbackError } = await supabase
        .from('feedback')
        .delete()
        .eq('teacher_id', teacherId);

      if (feedbackError) throw feedbackError;

      // מחק את המורה
      const { error: teacherError } = await supabase
        .from('teachers')
        .delete()
        .eq('id', teacherId);

      if (teacherError) throw teacherError;

      return true;
    } catch (error) {
      console.error('❌ שגיאה במחיקת מורה:', error);
      return false;
    }
  }

  // ---------- האזנה לשינויים בזמן אמת ----------
  function setupRealtimeUpdates() {
    // האזן לשינויים במשובים
    supabase
      .channel('feedback-changes')
      .on('postgres_changes', 
        { event: '*', schema: 'public', table: 'feedback' },
        (payload) => {
          console.log('שינוי במשובים:', payload);
          loadData(); // טען מחדש את הנתונים
        }
      )
      .subscribe();

    // האזן לשינויים במורים
    supabase
      .channel('teachers-changes')
      .on('postgres_changes', 
        { event: '*', schema: 'public', table: 'teachers' },
        (payload) => {
          console.log('שינוי במורים:', payload);
          loadData(); // טען מחדש את הנתונים
        }
      )
      .subscribe();
  }

  function updateSyncStatus(isOnline) {
    const syncStatus = document.getElementById('sync-status');
    if (syncStatus) {
      if (isOnline) {
        syncStatus.className = 'sync-status sync-online';
        syncStatus.innerHTML = '<i class="fas fa-database"></i><span>מחובר - נתונים משותפים</span>';
      } else {
        syncStatus.className = 'sync-status sync-offline';
        syncStatus.innerHTML = '<i class="fas fa-cloud-slash"></i><span>לא מחובר - נתונים מקומיים בלבד</span>';
      }
    }
  }

  // ---------- פונקציות עזר ----------
  function showScreen(name) {
    document.querySelectorAll(".screen").forEach(s => {
      s.classList.toggle("active", s.id === "screen-" + name);
    });
    appState.currentScreen = name;

    if (name === "teachers") renderTeacherList();
    if (name === "teacher-profile") renderTeacherProfile();
    if (name === "feedback") renderFeedbackScreen();
    if (name === "reports") renderReportsScreen();
    if (name === "leaderboard") renderLeaderboardScreen();
    if (name === "admin") renderAdminScreen();
  }

  function getTeacherById(id) {
    return teachers.find(t => t.id === id);
  }

  function getTeacherStats(id) {
    const entries = feedbackEntries.filter(f => f.teacher_id === id);
    const compliments = entries.filter(f => f.type === "compliment").length;
    const remarks = entries.filter(f => f.type === "remark").length;
    const score = compliments - remarks;
    return { compliments, remarks, score };
  }

  function getDisplayNameForUser(user) {
    return user?.username || "תלמיד";
  }

  function formatDateShort(date) {
    return new Date(date).toLocaleDateString("he-IL");
  }

  // ---------- Login ----------
  function handleLogin() {
    const username = document.getElementById("login-username").value.trim();
    const password = document.getElementById("login-password").value.trim();

    if (!username) {
      alert("צריך לכתוב שם משתמש.");
      return;
    }

    let user = { username, role: "student" };
    
    // אדמין
    if (username === "adir" && password === "1234") {
      user.role = "admin";
    }

    appState.currentUser = user;
    
    // שמור את פרטי המשתמש ב-Cookie למשך 30 יום
    setCookie('teacherFeedbackUser', JSON.stringify(user), 30);
    
    // עדכון UI
    document.getElementById("user-chip").innerHTML = `<i class="fas fa-user"></i><span>מחובר: ${user.role === "admin" ? "אדמין" : "תלמיד"}</span>`;
    document.getElementById("admin-tile").style.display = user.role === "admin" ? "block" : "none";
    
    showScreen("home");
  }

  function handleLogout() {
    appState.currentUser = null;
    // מחק את ה-Cookie
    deleteCookie('teacherFeedbackUser');
    document.getElementById("user-chip").innerHTML = '<i class="fas fa-user"></i><span>לא מחובר</span>';
    document.getElementById("admin-tile").style.display = "none";
    showScreen("login");
  }

  // ---------- טעינה אוטומטית של משתמש ----------
  function autoLoginUser() {
    const userCookie = getCookie('teacherFeedbackUser');
    if (userCookie) {
      try {
        const user = JSON.parse(userCookie);
        appState.currentUser = user;
        
        // עדכון UI
        document.getElementById("user-chip").innerHTML = `<i class="fas fa-user"></i><span>מחובר: ${user.role === "admin" ? "אדמין" : "תלמיד"}</span>`;
        document.getElementById("admin-tile").style.display = user.role === "admin" ? "block" : "none";
        
        // מעבר אוטומטי למסך הבית
        showScreen("home");
        
        console.log('✅ המשתמש נטען אוטומטית מ-Cookie');
      } catch (error) {
        console.error('❌ שגיאה בטעינת המשתמש מ-Cookie:', error);
        // במקרה של שגיאה, מחק את ה-Cookie הפגום
        deleteCookie('teacherFeedbackUser');
      }
    }
  }

  // ---------- Render Functions ----------
  function renderTeacherList() {
    const container = document.getElementById("teacher-list");
    const searchValue = document.getElementById("teacher-search").value.trim().toLowerCase();
    
    const filtered = teachers.filter(t => {
      if (!searchValue) return true;
      return t.name.toLowerCase().includes(searchValue) || t.subject.toLowerCase().includes(searchValue);
    });

    container.innerHTML = "";

    if (filtered.length === 0) {
      container.innerHTML = "<div class='feedback-empty'>לא נמצאו מורים.</div>";
      return;
    }

    filtered.forEach(t => {
      const stats = getTeacherStats(t.id);
      const card = document.createElement("div");
      card.className = "teacher-card";
      card.innerHTML = `
        <div class="teacher-main">
          <div class="teacher-name">${t.name}</div>
          <div class="teacher-subject">${t.subject}</div>
        </div>
        <div class="teacher-meta">
          <div class="feedback-counts">
            <div class="count-pill pos"><i class="fas fa-heart"></i> ${stats.compliments}</div>
            <div class="count-pill neg"><i class="fas fa-exclamation-triangle"></i> ${stats.remarks}</div>
          </div>
          <div style="font-size:12px;color:#94a3b8;">ניקוד: ${stats.score > 0 ? '+' : ''}${stats.score}</div>
        </div>
      `;
      card.addEventListener("click", () => {
        appState.selectedTeacherId = t.id;
        showScreen("teacher-profile");
      });
      container.appendChild(card);
    });
  }

  function renderTeacherProfile() {
    const teacher = getTeacherById(appState.selectedTeacherId);
    if (!teacher) return;

    document.getElementById("teacher-profile-name").textContent = teacher.name;
    
    const stats = getTeacherStats(teacher.id);
    document.getElementById("stat-compliments").textContent = stats.compliments;
    document.getElementById("stat-remarks").textContent = stats.remarks;
    document.getElementById("stat-behavior").textContent = stats.score > 0 ? '+' + stats.score : stats.score;

    const entries = feedbackEntries
      .filter(f => f.teacher_id === teacher.id)
      .sort((a, b) => new Date(b.created_at) - new Date(a.created_at));

    const listEl = document.getElementById("teacher-feedback-list");
    const emptyEl = document.getElementById("teacher-feedback-empty");
    
    listEl.innerHTML = "";
    
    if (entries.length === 0) {
      emptyEl.style.display = "block";
      listEl.style.display = "none";
      return;
    }

    emptyEl.style.display = "none";
    listEl.style.display = "flex";

    entries.slice(0, 10).forEach(e => {
      const item = document.createElement("div");
      item.className = "feedback-item";
      item.innerHTML = `
        <div>${e.text || "<i>אין טקסט</i>"}</div>
        <div class="feedback-tags">
          ${e.tags ? e.tags.map(tag => `<span class="tag-pill">${tag}</span>`).join("") : ''}
        </div>
        <div class="feedback-meta-line">
          <span>${e.type === "compliment" ? "👍 מחמאה" : "⚠️ הערה"} - ${e.user_name}</span>
          <span>${formatDateShort(e.created_at)}</span>
        </div>
      `;
      listEl.appendChild(item);
    });
  }

  function renderFeedbackScreen() {
    const teacher = getTeacherById(appState.selectedTeacherId);
    if (!teacher) return;

    const type = appState.feedbackType;
    const isCompliment = type === "compliment";

    document.getElementById("feedback-title").textContent = 
      `${isCompliment ? "הוספת מחמאה" : "הוספת הערה"} למורה ${teacher.name}`;
    
    document.getElementById("feedback-subtitle").textContent = 
      isCompliment ? "תכתוב מה היה טוב" : "תשמור על כנות";

    const quickTagsEl = document.getElementById("quick-tags");
    quickTagsEl.innerHTML = "";
    
    const tags = quickTagSets[type];
    tags.forEach(tag => {
      const btn = document.createElement("button");
      btn.type = "button";
      btn.className = "quick-tag";
      btn.textContent = tag;
      btn.addEventListener("click", () => {
        const selectedClass = isCompliment ? "selected-positive" : "selected-negative";
        btn.classList.toggle(selectedClass);
      });
      quickTagsEl.appendChild(btn);
    });

    const submitBtn = document.getElementById("feedback-submit-button");
    submitBtn.textContent = isCompliment ? "שליחת מחמאה" : "שליחת הערה";
    submitBtn.className = `btn btn-full ${isCompliment ? "btn-green" : "btn-red"}`;
    
    document.getElementById("feedback-text").value = "";
  }

  function renderReportsScreen() {
    const userEntries = feedbackEntries.filter(f => f.user_name === getDisplayNameForUser(appState.currentUser));
    const totalCompliments = userEntries.filter(f => f.type === "compliment").length;
    const totalRemarks = userEntries.filter(f => f.type === "remark").length;

    document.getElementById("reports-total-compliments").textContent = totalCompliments;
    document.getElementById("reports-total-remarks").textContent = totalRemarks;

    // נתונים שבועיים
    const weekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000);
    const weekEntries = userEntries.filter(f => new Date(f.created_at) >= weekAgo);
    const weekCompliments = weekEntries.filter(f => f.type === "compliment").length;
    const weekRemarks = weekEntries.filter(f => f.type === "remark").length;

    document.getElementById("reports-week-compliments").textContent = weekCompliments;
    document.getElementById("reports-week-remarks").textContent = weekRemarks;

    const weekSummary = document.getElementById("reports-week-summary-text");
    if (weekEntries.length === 0) {
      weekSummary.textContent = "אין נתונים לשבוע הזה.";
    } else if (weekCompliments > weekRemarks) {
      weekSummary.textContent = "שבוע חיובי! יותר מחמאות מהערות.";
    } else {
      weekSummary.textContent = "שבוע ביקורתי. יותר הערות ממחמאות.";
    }

    // פעילות אחרונה
    const latestContainer = document.getElementById("reports-latest-list");
    const emptyEl = document.getElementById("reports-empty");
    
    latestContainer.innerHTML = "";
    
    if (userEntries.length === 0) {
      emptyEl.style.display = "block";
      latestContainer.style.display = "none";
      return;
    }

    emptyEl.style.display = "none";
    latestContainer.style.display = "flex";

    const latest = userEntries
      .sort((a, b) => new Date(b.created_at) - new Date(a.created_at))
      .slice(0, 5);

    latest.forEach(e => {
      const teacher = getTeacherById(e.teacher_id);
      const item = document.createElement("div");
      item.className = "feedback-item";
      item.innerHTML = `
        <div>${e.text || "<i>אין טקסט</i>"}</div>
        <div class="feedback-tags">
          ${e.tags ? e.tags.map(tag => `<span class="tag-pill">${tag}</span>`).join("") : ''}
        </div>
        <div class="feedback-meta-line">
          <span>${e.type === "compliment" ? "👍" : "⚠️"} ${teacher?.name || "מורה"}</span>
          <span>${formatDateShort(e.created_at)}</span>
        </div>
      `;
      latestContainer.appendChild(item);
    });
  }

  function renderLeaderboardScreen() {
    const tbody = document.getElementById("leaderboard-body");
    tbody.innerHTML = "";

    const entries = Object.keys(studentStats).map(name => {
      const stats = studentStats[name];
      const total = stats.compliments + stats.remarks;
      return { name, ...stats, total };
    }).sort((a, b) => b.total - a.total);

    if (entries.length === 0) {
      tbody.innerHTML = `
        <tr>
          <td colspan="6" style="text-align:center;padding:20px;color:#94a3b8;">
            <i class="fas fa-trophy"></i> אין עדיין נתונים
          </td>
        </tr>
      `;
      return;
    }

    const maxTotal = Math.max(...entries.map(e => e.total));

    entries.forEach((e, index) => {
      const widthPercent = Math.max(5, (e.total / maxTotal) * 100);
      const tr = document.createElement("tr");
      tr.innerHTML = `
        <td>${index + 1}</td>
        <td>${e.name}</td>
        <td>${e.compliments}</td>
        <td>${e.remarks}</td>
        <td>${e.total}</td>
        <td>
          <div class="leaderboard-bar-wrapper">
            <div class="leaderboard-bar" style="width:${widthPercent}%"></div>
          </div>
        </td>
      `;
      tbody.appendChild(tr);
    });
  }

  function renderAdminScreen() {
    const container = document.getElementById("admin-teacher-list");
    container.innerHTML = "";

    if (teachers.length === 0) {
      container.innerHTML = "<div class='feedback-empty'>אין מורים ברשימה.</div>";
      return;
    }

    teachers.forEach(t => {
      const stats = getTeacherStats(t.id);
      const card = document.createElement("div");
      card.className = "teacher-card";
      card.innerHTML = `
        <div class="teacher-main">
          <div class="teacher-name">${t.name}</div>
          <div class="teacher-subject">${t.subject}</div>
          <div style="font-size:12px;color:#94a3b8;">משובים: ${stats.compliments + stats.remarks}</div>
        </div>
        <div class="teacher-meta">
          <button class="btn btn-red btn-small" data-delete-id="${t.id}">
            <i class="fas fa-trash"></i>
            <span>מחיקה</span>
          </button>
        </div>
      `;
      
      const deleteBtn = card.querySelector("button[data-delete-id]");
      deleteBtn.addEventListener("click", async (e) => {
        e.stopPropagation();
        if (confirm(`למחוק את המורה ${t.name}? כל המשובים עליו יימחקו.`)) {
          const success = await deleteTeacher(t.id);
          if (success) {
            await loadData();
            renderAdminScreen();
            renderTeacherList();
          } else {
            alert("הייתה בעיה במחיקת המורה.");
          }
        }
      });
      
      container.appendChild(card);
    });
  }

  // ---------- Event Listeners ----------
  document.addEventListener("DOMContentLoaded", async () => {
    // טעינת נתונים
    await loadData();
    setupRealtimeUpdates();

    // נסה לטעון את המשתמש אוטומטית מ-Cookie
    autoLoginUser();

    // Login
    document.getElementById("login-button").addEventListener("click", handleLogin);
    document.getElementById("login-password").addEventListener("keydown", (e) => {
      if (e.key === "Enter") handleLogin();
    });

    // Logout
    document.getElementById("user-chip").addEventListener("click", () => {
      if (appState.currentUser && confirm("להתנתק?")) {
        handleLogout();
      }
    });

    // Tile clicks
    document.querySelectorAll(".tile").forEach(tile => {
      tile.addEventListener("click", () => {
        const action = tile.dataset.action;
        if (action === "view-teachers") {
          showScreen("teachers");
        } else if (action === "quick-compliment") {
          appState.feedbackType = "compliment";
          showScreen("teachers");
        } else if (action === "quick-remark") {
          appState.feedbackType = "remark";
          showScreen("teachers");
        } else if (action === "view-reports") {
          showScreen("reports");
        } else if (action === "leaderboard") {
          showScreen("leaderboard");
        } else if (action === "admin") {
          if (appState.currentUser?.role === "admin") {
            showScreen("admin");
          } else {
            alert("רק אדמין יכול להיכנס לכאן.");
          }
        }
      });
    });

    // Back links
    document.querySelectorAll(".back-link").forEach(link => {
      link.addEventListener("click", () => {
        const target = link.dataset.backTo;
        if (target) showScreen(target);
      });
    });

    // Search
    document.getElementById("teacher-search").addEventListener("input", renderTeacherList);

    // Teacher profile buttons
    document.getElementById("btn-profile-compliment").addEventListener("click", () => {
      appState.feedbackType = "compliment";
      showScreen("feedback");
    });
    
    document.getElementById("btn-profile-remark").addEventListener("click", () => {
      appState.feedbackType = "remark";
      showScreen("feedback");
    });

    // Feedback back
    document.getElementById("feedback-back").addEventListener("click", () => {
      showScreen("teacher-profile");
    });

    // Submit feedback
    document.getElementById("feedback-submit-button").addEventListener("click", async () => {
      const teacherId = appState.selectedTeacherId;
      const type = appState.feedbackType;
      
      if (!teacherId || !type) {
        alert("משהו נתקע. תנסה שוב.");
        return;
      }

      const teacher = getTeacherById(teacherId);
      const text = document.getElementById("feedback-text").value.trim();
      
      const quickTagsWrap = document.getElementById("quick-tags");
      const tagButtons = Array.from(quickTagsWrap.querySelectorAll(".quick-tag"));
      const tags = tagButtons.filter(btn => 
        btn.classList.contains("selected-positive") || 
        btn.classList.contains("selected-negative")
      ).map(btn => btn.textContent);

      if (tags.length === 0 && !text) {
        alert("תבחר תגית או תכתוב משהו.");
        return;
      }

      const userName = getDisplayNameForUser(appState.currentUser);

      const success = await saveFeedback({
        teacherId,
        type,
        tags,
        text,
        user: userName
      });

      if (success) {
        alert(`${type === "compliment" ? "מחמאה" : "הערה"} נשמרה בהצלחה!`);
        showScreen("teacher-profile");
      } else {
        alert("הייתה בעיה בשמירה, נסה שוב.");
      }
    });

    // Admin - Add teacher
    document.getElementById("admin-add-teacher").addEventListener("click", async () => {
      const nameInput = document.getElementById("admin-new-name");
      const subjectInput = document.getElementById("admin-new-subject");
      const name = nameInput.value.trim();
      const subject = subjectInput.value.trim();

      if (!name || !subject) {
        alert("צריך למלא גם שם וגם מקצוע.");
        return;
      }

      const newTeacher = await addTeacher(name, subject);
      
      if (newTeacher) {
        nameInput.value = "";
        subjectInput.value = "";
        await loadData();
        renderAdminScreen();
        renderTeacherList();
        alert("המורה נוסף בהצלחה!");
      } else {
        alert("הייתה בעיה בהוספת המורה.");
      }
    });

    // Help button
    document.getElementById("help-button").addEventListener("click", () => {
      alert("מערכת משוב למורים\n\n• התחברות עם כל שם משתמש\n• adir/1234 לאדמין\n• כל המשובים נשמרים ב-Supabase ונראים לכולם\n• המערכת זוכרת אותך אוטומטית בפעם הבאה!");
    });

    // Notifications
    document.getElementById("notif-button").addEventListener("click", () => {
      const panel = document.getElementById("notif-panel");
      panel.classList.toggle("open");
    });
  });
</script>
</body>
</html>