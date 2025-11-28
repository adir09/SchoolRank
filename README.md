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
    :root {
      --bg: #0f172a;
      --bg-alt: #1e293b;
      --card-bg: #1e293b;
      --card-alt: #334155;
      --green-dark: #059669;
      --green-light: #10b981;
      --red: #ef4444;
      --orange: #f59e0b;
      --blue: #3b82f6;
      --purple: #8b5cf6;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --border-soft: #475569;
      --shadow-soft: 0 4px 6px -1px rgba(0, 0, 0, 0.3), 0 2px 4px -1px rgba(0, 0, 0, 0.2);
      --shadow-hover: 0 20px 25px -5px rgba(0, 0, 0, 0.4), 0 10px 10px -5px rgba(0, 0, 0, 0.3);
      --radius-card: 16px;
      --radius-small: 8px;
      --transition-fast: 0.2s ease-out;
      --transition-smooth: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%);
      color: var(--text-main);
      line-height: 1.6;
      min-height: 100vh;
      direction: rtl;
    }

    .app {
      max-width: 1200px;
      margin: 0 auto;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      background: rgba(15, 23, 42, 0.8);
    }

    .app-header {
      background: linear-gradient(90deg, #0c4a6e 0%, #075985 100%);
      color: #fff;
      padding: 12px 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
      position: sticky;
      top: 0;
      z-index: 100;
    }

    .app-header-left {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .logo {
      width: 36px;
      height: 36px;
      border-radius: 10px;
      background: linear-gradient(135deg, #10b981, #059669);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      color: white;
    }

    .app-title {
      font-size: 18px;
      font-weight: 700;
      background: linear-gradient(90deg, #e0f2fe, #bae6fd);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .app-header-right {
      display: flex;
      gap: 12px;
      align-items: center;
    }

    .icon-button {
      width: 40px;
      height: 40px;
      border-radius: 12px;
      border: 1px solid rgba(255, 255, 255, 0.15);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      font-size: 16px;
      background: rgba(255, 255, 255, 0.1);
      position: relative;
      transition: all var(--transition-fast);
      color: #e2e8f0;
    }

    .icon-button:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: translateY(-2px);
    }

    .notif-badge {
      position: absolute;
      top: -5px;
      right: -5px;
      min-width: 18px;
      height: 18px;
      border-radius: 999px;
      background: var(--red);
      color: #fff;
      font-size: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0 4px;
    }

    .user-chip {
      font-size: 13px;
      padding: 8px 16px;
      border-radius: 12px;
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid rgba(255, 255, 255, 0.15);
      cursor: pointer;
      transition: all var(--transition-fast);
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .user-chip:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: translateY(-2px);
    }

    .app-main {
      padding: 20px;
      flex: 1;
      display: flex;
      flex-direction: column;
    }

    .screen {
      display: none;
    }

    .screen.active {
      display: block;
    }

    .login-wrapper {
      max-width: 440px;
      margin: 60px auto;
      background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
      border-radius: 20px;
      padding: 30px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .login-title {
      font-size: 24px;
      font-weight: 700;
      margin-bottom: 8px;
      text-align: center;
      background: linear-gradient(90deg, #10b981, #3b82f6);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .login-sub {
      font-size: 14px;
      color: var(--text-muted);
      text-align: center;
      margin-bottom: 24px;
      line-height: 1.6;
    }

    .form-field {
      margin-bottom: 16px;
    }

    .form-label {
      font-size: 14px;
      margin-bottom: 6px;
      color: var(--text-muted);
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .text-input {
      width: 100%;
      padding: 12px 16px;
      border-radius: 12px;
      border: 1px solid var(--border-soft);
      background: rgba(15, 23, 42, 0.6);
      color: var(--text-main);
      font-size: 15px;
      outline: none;
      transition: all var(--transition-fast);
    }

    .text-input:focus {
      border-color: var(--green-light);
      box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2);
    }

    .btn {
      border-radius: 12px;
      border: none;
      padding: 12px 20px;
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: all var(--transition-fast);
    }

    .btn-full {
      width: 100%;
    }

    .btn-green {
      background: linear-gradient(135deg, #10b981, #059669);
      color: #fff;
    }

    .btn-green:hover {
      transform: translateY(-2px);
    }

    .btn-red {
      background: linear-gradient(135deg, #ef4444, #dc2626);
      color: #fff;
    }

    .btn-red:hover {
      transform: translateY(-2px);
    }

    .btn-blue {
      background: linear-gradient(135deg, #3b82f6, #2563eb);
      color: #fff;
    }

    .btn-blue:hover {
      transform: translateY(-2px);
    }

    .btn-small {
      padding: 8px 12px;
      font-size: 13px;
    }

    .greeting {
      font-size: 28px;
      font-weight: 700;
      margin-bottom: 8px;
      background: linear-gradient(90deg, #e0f2fe, #bae6fd);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .sub-greeting {
      font-size: 16px;
      color: var(--text-muted);
      margin-bottom: 24px;
    }

    .tiles-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
      gap: 20px;
    }

    .tile {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 20px;
      box-shadow: var(--shadow-soft);
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      cursor: pointer;
      transition: all var(--transition-smooth);
      border: 1px solid rgba(255, 255, 255, 0.05);
      min-height: 140px;
    }

    .tile:hover {
      transform: translateY(-5px);
      box-shadow: var(--shadow-hover);
    }

    .tile-header {
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      margin-bottom: 12px;
    }

    .tile-icon {
      width: 40px;
      height: 40px;
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      color: white;
    }

    .tile-primary .tile-icon { background: linear-gradient(135deg, #10b981, #059669); }
    .tile-danger .tile-icon { background: linear-gradient(135deg, #ef4444, #dc2626); }
    .tile-neutral .tile-icon { background: linear-gradient(135deg, #3b82f6, #2563eb); }
    .tile-admin .tile-icon { background: linear-gradient(135deg, #8b5cf6, #7c3aed); }

    .tile-title {
      font-size: 18px;
      font-weight: 700;
      margin-bottom: 8px;
      color: #fff;
    }

    .tile-desc {
      font-size: 14px;
      color: var(--text-muted);
      margin-bottom: 16px;
      line-height: 1.5;
    }

    .section-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      margin-top: 10px;
    }

    .section-title {
      font-size: 24px;
      font-weight: 700;
      background: linear-gradient(90deg, #e0f2fe, #bae6fd);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .back-link {
      font-size: 14px;
      color: var(--green-light);
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: all var(--transition-fast);
      padding: 8px 12px;
      border-radius: 8px;
      background: rgba(16, 185, 129, 0.1);
    }

    .back-link:hover {
      color: #7bed9f;
      transform: translateY(-2px);
    }

    .search-bar {
      margin-bottom: 20px;
      position: relative;
    }

    .search-input {
      width: 100%;
      padding: 14px 20px 14px 50px;
      border-radius: 12px;
      border: 1px solid var(--border-soft);
      font-size: 15px;
      outline: none;
      background: rgba(15, 23, 42, 0.6);
      color: var(--text-main);
    }

    .search-icon {
      position: absolute;
      top: 50%;
      right: 16px;
      transform: translateY(-50%);
      color: var(--text-muted);
    }

    .teacher-list {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .teacher-card {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 16px 20px;
      box-shadow: var(--shadow-soft);
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      transition: all var(--transition-smooth);
      border: 1px solid rgba(255, 255, 255, 0.05);
    }

    .teacher-card:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
    }

    .teacher-main {
      display: flex;
      flex-direction: column;
      gap: 4px;
    }

    .teacher-name {
      font-size: 16px;
      font-weight: 600;
    }

    .teacher-subject {
      font-size: 14px;
      color: var(--text-muted);
    }

    .teacher-meta {
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      gap: 6px;
    }

    .feedback-counts {
      display: flex;
      gap: 8px;
      font-size: 12px;
      align-items: center;
    }

    .count-pill {
      padding: 4px 10px;
      border-radius: 20px;
      font-weight: 600;
      display: inline-flex;
      align-items: center;
      gap: 4px;
      font-size: 12px;
    }

    .count-pill.pos {
      background: rgba(16, 185, 129, 0.15);
      color: #10b981;
    }

    .count-pill.neg {
      background: rgba(239, 68, 68, 0.15);
      color: #ef4444;
    }

    .stat-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 16px;
      margin: 20px 0 24px;
    }

    .stat-card {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 20px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.05);
      text-align: center;
    }

    .stat-label {
      font-size: 14px;
      color: var(--text-muted);
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
    }

    .stat-value {
      font-size: 32px;
      font-weight: 700;
      margin-bottom: 4px;
    }

    .stat-compliments .stat-value { color: #10b981; }
    .stat-remarks .stat-value { color: #ef4444; }
    .stat-behavior .stat-value { color: #3b82f6; }

    .button-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 16px;
      margin-bottom: 20px;
    }

    .feedback-list {
      display: flex;
      flex-direction: column;
      gap: 12px;
      margin-top: 16px;
    }

    .feedback-empty {
      font-size: 14px;
      color: var(--text-muted);
      margin-top: 20px;
      text-align: center;
      padding: 30px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: var(--radius-card);
    }

    .feedback-item {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 16px;
      box-shadow: var(--shadow-soft);
      font-size: 14px;
      display: flex;
      flex-direction: column;
      gap: 8px;
      border: 1px solid rgba(255, 255, 255, 0.05);
    }

    .feedback-meta-line {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      font-size: 12px;
      color: var(--text-muted);
    }

    .feedback-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
    }

    .tag-pill {
      padding: 4px 10px;
      border-radius: 20px;
      background: rgba(255, 255, 255, 0.1);
      font-size: 11px;
      color: #ddd;
    }

    .feedback-header {
      margin-bottom: 20px;
      padding: 20px;
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.05);
    }

    .feedback-header-title {
      font-size: 20px;
      font-weight: 600;
      margin-bottom: 6px;
    }

    .feedback-header-sub {
      font-size: 14px;
      color: var(--text-muted);
    }

    .quick-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin: 20px 0;
    }

    .quick-tag {
      font-size: 13px;
      padding: 8px 16px;
      border-radius: 20px;
      border: 1px solid var(--border-soft);
      background: rgba(15, 23, 42, 0.6);
      cursor: pointer;
      transition: all var(--transition-fast);
    }

    .quick-tag:hover {
      transform: translateY(-2px);
    }

    .quick-tag.selected-positive {
      background: rgba(16, 185, 129, 0.2);
      border-color: #10b981;
      color: #10b981;
    }

    .quick-tag.selected-negative {
      background: rgba(239, 68, 68, 0.2);
      border-color: #ef4444;
      color: #ef4444;
    }

    .textarea {
      width: 100%;
      min-height: 120px;
      resize: vertical;
      border-radius: 12px;
      border: 1px solid var(--border-soft);
      padding: 16px;
      font-size: 15px;
      outline: none;
      margin-bottom: 16px;
      background: rgba(15, 23, 42, 0.6);
      color: var(--text-main);
      font-family: inherit;
    }

    .sync-status {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 12px;
      padding: 8px 12px;
      border-radius: 8px;
      margin-top: 8px;
    }

    .sync-online {
      background: rgba(16, 185, 129, 0.1);
      color: #10b981;
    }

    .sync-offline {
      background: rgba(245, 158, 11, 0.1);
      color: #f59e0b;
    }

    .notif-panel {
      position: fixed;
      top: 70px;
      right: 20px;
      width: min(380px, 90vw);
      max-height: 70vh;
      background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
      border-radius: 16px;
      box-shadow: 0 20px 40px rgba(0,0,0,0.4);
      border: 1px solid rgba(255,255,255,0.1);
      padding: 16px;
      display: flex;
      flex-direction: column;
      opacity: 0;
      transform: translateY(-10px);
      pointer-events: none;
      transition: opacity 0.3s ease-out, transform 0.3s ease-out;
      z-index: 1000;
    }

    .notif-panel.open {
      opacity: 1;
      transform: translateY(0);
      pointer-events: auto;
    }

    .notif-header {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 6px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .notif-sub {
      font-size: 13px;
      color: var(--text-muted);
      margin-bottom: 12px;
    }

    .notif-list {
      overflow-y: auto;
      padding-right: 4px;
    }

    .notif-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      padding: 12px;
      font-size: 13px;
      margin-bottom: 8px;
      border: 1px solid rgba(255,255,255,0.03);
    }

    .notif-empty {
      font-size: 13px;
      color: var(--text-muted);
      padding: 20px;
      text-align: center;
    }

    .admin-grid {
      display: grid;
      grid-template-columns: minmax(0, 1.2fr) minmax(0, 1fr);
      gap: 20px;
      margin-top: 20px;
    }

    .admin-panel {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 20px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.05);
    }

    .admin-panel-title {
      font-size: 18px;
      font-weight: 600;
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .admin-form-row {
      display: flex;
      gap: 12px;
      margin-bottom: 16px;
    }

    .admin-note {
      font-size: 12px;
      color: var(--text-muted);
      margin-top: 8px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .reports-section {
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .report-summary-card {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 20px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.05);
    }

    .report-row {
      display: flex;
      justify-content: space-between;
      font-size: 14px;
      margin-top: 8px;
      color: var(--text-muted);
      padding: 6px 0;
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .report-highlight {
      margin-top: 12px;
      font-size: 14px;
      color: #e2e8f0;
      padding: 12px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
    }

    .leaderboard-table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 12px;
      font-size: 14px;
    }

    .leaderboard-table th,
    .leaderboard-table td {
      padding: 10px 12px;
      text-align: right;
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .leaderboard-table th {
      font-size: 13px;
      color: #cbd5e1;
      background: rgba(255, 255, 255, 0.05);
      position: sticky;
      top: 0;
      font-weight: 600;
    }

    .leaderboard-bar-wrapper {
      width: 100%;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 999px;
      overflow: hidden;
      height: 8px;
    }

    .leaderboard-bar {
      height: 100%;
      background: linear-gradient(90deg, #10b981, #3b82f6);
      border-radius: 999px;
    }

    .feedback-actions {
      display: flex;
      gap: 8px;
      margin-top: 8px;
    }

    .delete-feedback-btn {
      background: rgba(239, 68, 68, 0.2);
      color: #ef4444;
      border: 1px solid rgba(239, 68, 68, 0.3);
      border-radius: 6px;
      padding: 4px 8px;
      font-size: 11px;
      cursor: pointer;
      transition: all 0.2s;
    }

    .delete-feedback-btn:hover {
      background: rgba(239, 68, 68, 0.3);
    }

    /* אנימציות חדשות */
    @keyframes slideInUp {
      from {
        opacity: 0;
        transform: translateY(30px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @keyframes fadeOut {
      from {
        opacity: 1;
      }
      to {
        opacity: 0;
      }
    }

    @keyframes pulse {
      0% {
        transform: scale(1);
      }
      50% {
        transform: scale(1.05);
      }
      100% {
        transform: scale(1);
      }
    }

    @keyframes checkmark {
      0% {
        transform: scale(0);
        opacity: 0;
      }
      50% {
        transform: scale(1.2);
      }
      100% {
        transform: scale(1);
        opacity: 1;
      }
    }

    .feedback-toast {
      position: fixed;
      top: 100px;
      right: 50%;
      transform: translateX(50%);
      background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
      color: white;
      padding: 16px 24px;
      border-radius: 12px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.3);
      border: 1px solid rgba(255,255,255,0.1);
      display: flex;
      align-items: center;
      gap: 12px;
      z-index: 10000;
      animation: slideInUp 0.5s ease-out;
      max-width: 400px;
      width: 90%;
    }

    .feedback-toast.fade-out {
      animation: fadeOut 0.5s ease-out forwards;
    }

    .feedback-toast.compliment {
      border-left: 4px solid #10b981;
    }

    .feedback-toast.remark {
      border-left: 4px solid #ef4444;
    }

    .toast-icon {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      animation: checkmark 0.6s ease-out;
    }

    .toast-icon.compliment {
      background: rgba(16, 185, 129, 0.2);
      color: #10b981;
    }

    .toast-icon.remark {
      background: rgba(239, 68, 68, 0.2);
      color: #ef4444;
    }

    .toast-content {
      flex: 1;
    }

    .toast-title {
      font-weight: 600;
      margin-bottom: 4px;
    }

    .toast-message {
      font-size: 14px;
      opacity: 0.9;
    }

    .pulse-animation {
      animation: pulse 0.6s ease-in-out;
    }

    .shake-animation {
      animation: shake 0.5s ease-in-out;
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-5px); }
      75% { transform: translateX(5px); }
    }

    .floating-particles {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 9999;
    }

    .particle {
      position: absolute;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      pointer-events: none;
    }

    .particle.heart {
      background: #ef4444;
      animation: floatUp 1.5s ease-out forwards;
    }

    .particle.star {
      background: #f59e0b;
      animation: floatUp 2s ease-out forwards;
    }

    .particle.sparkle {
      background: #3b82f6;
      animation: floatUp 1.8s ease-out forwards;
    }

    @keyframes floatUp {
      0% {
        transform: translateY(0) rotate(0deg);
        opacity: 1;
      }
      100% {
        transform: translateY(-100px) rotate(180deg);
        opacity: 0;
      }
    }
  </style>
</head>
<body>
<div class="app">
  <!-- Floating Particles Container -->
  <div id="floating-particles" class="floating-particles"></div>

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

      <!-- New Admin Section: Manage Feedback -->
      <div class="admin-panel" style="margin-top: 20px;">
        <div class="admin-panel-title">
          <i class="fas fa-comments"></i>
          <span>ניהול משובים</span>
        </div>
        <div class="search-bar">
          <input id="admin-feedback-search" class="search-input" type="text" placeholder="חיפוש משובים...">
          <div class="search-icon"><i class="fas fa-search"></i></div>
        </div>
        <div id="admin-feedback-list" class="feedback-list"></div>
        <div id="admin-feedback-empty" class="feedback-empty">
          <i class="fas fa-inbox"></i>
          <div>אין משובים.</div>
        </div>
      </div>
    </section>

  </main>
</div>

<script>
  // ---------- הגדרות Supabase ----------
  const SUPABASE_URL = 'https://xidfthnboggokcsloglt.supabase.co';
  const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhpZGZ0aG5ib2dnb2tjc2xvZ2x0Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjQzMTIwNzUsImV4cCI6MjA3OTg4ODA3NX0.fEPS2FJYlcZ4DOv7I0RBEcwZBfT0MdRGslk9cp_2GwU';

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

  // ---------- Animation Functions ----------
function showFeedbackAnimation(type, teacherName) {
  // Create toast notification
  const toast = document.createElement('div');
  toast.className = `feedback-toast ${type}`;
  
  const iconClass = type === 'compliment' ? 'fas fa-heart' : 'fas fa-exclamation-triangle';
  const title = type === 'compliment' ? 'מחמאה נשלחה!' : 'הערה נשלחה';
  const message = type === 'compliment' 
    ? `המחמאה למורה ${teacherName} נשמרה בהצלחה`
    : `הוסך הערה למורה ${teacherName}`;  // שיניתי כאן
  
  toast.innerHTML = `
    <div class="toast-icon ${type}">
      <i class="${iconClass}"></i>
    </div>
    <div class="toast-content">
      <div class="toast-title">${title}</div>
      <div class="toast-message">${message}</div>
    </div>
  `;
  
  // ... rest of the function remains the same
}
    
    document.body.appendChild(toast);
    
    // Add pulse animation to the submit button
    const submitBtn = document.getElementById('feedback-submit-button');
    if (submitBtn) {
      submitBtn.classList.add('pulse-animation');
      setTimeout(() => {
        submitBtn.classList.remove('pulse-animation');
      }, 600);
    }
    
    // Create floating particles for compliments
    if (type === 'compliment') {
      createFloatingParticles();
    }
    
    // Remove toast after 3 seconds
    setTimeout(() => {
      toast.classList.add('fade-out');
      setTimeout(() => {
        if (toast.parentNode) {
          toast.parentNode.removeChild(toast);
        }
      }, 500);
    }, 3000);
  }
  
  function createFloatingParticles() {
    const container = document.getElementById('floating-particles');
    const types = ['heart', 'star', 'sparkle'];
    
    // Create 15 particles
    for (let i = 0; i < 15; i++) {
      setTimeout(() => {
        const particle = document.createElement('div');
        const type = types[Math.floor(Math.random() * types.length)];
        
        particle.className = `particle ${type}`;
        particle.style.left = `${Math.random() * 100}%`;
        particle.style.top = `${80 + Math.random() * 20}%`;
        particle.style.animationDelay = `${Math.random() * 0.5}s`;
        
        container.appendChild(particle);
        
        // Remove particle after animation
        setTimeout(() => {
          if (particle.parentNode) {
            particle.parentNode.removeChild(particle);
          }
        }, 2000);
      }, i * 100);
    }
  }
  
  function showErrorAnimation() {
    const submitBtn = document.getElementById('feedback-submit-button');
    if (submitBtn) {
      submitBtn.classList.add('shake-animation');
      setTimeout(() => {
        submitBtn.classList.remove('shake-animation');
      }, 500);
    }
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

  async function deleteFeedback(feedbackId) {
    try {
      const { error } = await supabase
        .from('feedback')
        .delete()
        .eq('id', feedbackId);

      if (error) throw error;
      return true;
    } catch (error) {
      console.error('❌ שגיאה במחיקת משוב:', error);
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
      
      // אם המשתמש הוא אדמין, הוסף כפתור מחיקה
      const deleteButton = appState.currentUser?.role === "admin" ? 
        `<button class="delete-feedback-btn" data-feedback-id="${e.id}">
          <i class="fas fa-trash"></i> מחק
        </button>` : '';
      
      item.innerHTML = `
        <div>${e.text || "<i>אין טקסט</i>"}</div>
        <div class="feedback-tags">
          ${e.tags ? e.tags.map(tag => `<span class="tag-pill">${tag}</span>`).join("") : ''}
        </div>
        <div class="feedback-meta-line">
          <span>${e.type === "compliment" ? "👍 מחמאה" : "⚠️ הערה"} - ${e.user_name}</span>
          <span>${formatDateShort(e.created_at)}</span>
        </div>
        ${deleteButton ? `<div class="feedback-actions">${deleteButton}</div>` : ''}
      `;
      
      // הוסף מאזין לכפתור המחיקה אם קיים
      if (appState.currentUser?.role === "admin") {
        const deleteBtn = item.querySelector('.delete-feedback-btn');
        deleteBtn.addEventListener('click', async (event) => {
          event.stopPropagation();
          const feedbackId = parseInt(deleteBtn.dataset.feedbackId);
          if (confirm('למחוק את המשוב?')) {
            const success = await deleteFeedback(feedbackId);
            if (success) {
              await loadData();
              renderTeacherProfile();
            }
          }
        });
      }
      
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

    // Render feedback management section
    renderAdminFeedbackList();
  }

  function renderAdminFeedbackList() {
    const container = document.getElementById("admin-feedback-list");
    const searchValue = document.getElementById("admin-feedback-search")?.value.trim().toLowerCase() || '';
    
    const filtered = feedbackEntries.filter(f => {
      if (!searchValue) return true;
      const teacher = getTeacherById(f.teacher_id);
      const teacherName = teacher ? teacher.name.toLowerCase() : '';
      const userName = f.user_name.toLowerCase();
      const text = f.text ? f.text.toLowerCase() : '';
      
      return teacherName.includes(searchValue) || 
             userName.includes(searchValue) || 
             text.includes(searchValue);
    });

    container.innerHTML = "";

    if (filtered.length === 0) {
      document.getElementById("admin-feedback-empty").style.display = "block";
      container.style.display = "none";
      return;
    }

    document.getElementById("admin-feedback-empty").style.display = "none";
    container.style.display = "flex";

    filtered.slice(0, 20).forEach(e => {
      const teacher = getTeacherById(e.teacher_id);
      const item = document.createElement("div");
      item.className = "feedback-item";
      item.innerHTML = `
        <div><strong>${teacher?.name || "מורה לא ידוע"} - ${e.type === "compliment" ? "מחמאה" : "הערה"}</strong></div>
        <div>${e.text || "<i>אין טקסט</i>"}</div>
        <div class="feedback-tags">
          ${e.tags ? e.tags.map(tag => `<span class="tag-pill">${tag}</span>`).join("") : ''}
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
      
      const deleteBtn = item.querySelector('.delete-feedback-btn');
      deleteBtn.addEventListener('click', async () => {
        if (confirm('למחוק את המשוב?')) {
          const success = await deleteFeedback(e.id);
          if (success) {
            await loadData();
            renderAdminFeedbackList();
            if (appState.currentScreen === "teacher-profile") {
              renderTeacherProfile();
            }
          }
        }
      });
      
      container.appendChild(item);
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

    // Admin feedback search
    document.getElementById("admin-feedback-search")?.addEventListener("input", renderAdminFeedbackList);

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

    // ---------- Modified Submit Feedback Function ----------
    document.getElementById("feedback-submit-button").addEventListener("click", async () => {
      const teacherId = appState.selectedTeacherId;
      const type = appState.feedbackType;
      
      if (!teacherId || !type) {
        showErrorAnimation();
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
        showErrorAnimation();
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
        // Show animation instead of alert
        showFeedbackAnimation(type, teacher.name);
        
        // Return to teacher profile after a short delay
        setTimeout(() => {
          showScreen("teacher-profile");
        }, 1000);
      } else {
        showErrorAnimation();
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
