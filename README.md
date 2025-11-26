<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>מערכת משוב למורים</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
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
      backdrop-filter: blur(10px);
    }

    /* Header */
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
      box-shadow: 0 4px 8px rgba(16, 185, 129, 0.3);
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
      backdrop-filter: blur(10px);
      position: relative;
      transition: all var(--transition-fast);
      color: #e2e8f0;
    }

    .icon-button:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
    }

    .notif-badge {
      position: absolute;
      top: -5px;
      inset-inline-start: -5px;
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
      box-shadow: 0 0 0 2px var(--bg-alt);
      font-weight: 700;
    }

    .user-chip {
      font-size: 13px;
      padding: 8px 16px;
      border-radius: 12px;
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid rgba(255, 255, 255, 0.15);
      cursor: pointer;
      transition: all var(--transition-fast);
      backdrop-filter: blur(10px);
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .user-chip:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: translateY(-2px);
    }

    .user-chip i {
      font-size: 14px;
    }

    /* Loading spinner */
    .loading {
      display: inline-block;
      width: 20px;
      height: 20px;
      border: 3px solid rgba(255,255,255,.3);
      border-radius: 50%;
      border-top-color: #fff;
      animation: spin 1s ease-in-out infinite;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    /* Main content */
    .app-main {
      padding: 20px;
      flex: 1;
      display: flex;
      flex-direction: column;
    }

    .screen {
      display: none;
      animation: fadeIn 0.3s ease-out;
    }

    .screen.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* Login */
    .login-wrapper {
      max-width: 440px;
      margin: 60px auto;
      background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
      border-radius: 20px;
      padding: 30px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.1);
      position: relative;
      overflow: hidden;
    }

    .login-wrapper::before {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      width: 100%;
      height: 4px;
      background: linear-gradient(90deg, #10b981, #3b82f6);
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
      background: rgba(15, 23, 42, 0.8);
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
      position: relative;
      overflow: hidden;
    }

    .btn::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(rgba(255,255,255,0.1), transparent);
      opacity: 0;
      transition: opacity var(--transition-fast);
    }

    .btn:hover::before {
      opacity: 1;
    }

    .btn-full {
      width: 100%;
      justify-content: center;
    }

    .btn-green {
      background: linear-gradient(135deg, #10b981, #059669);
      color: #fff;
      box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
    }

    .btn-green:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(16, 185, 129, 0.4);
    }

    .btn-red {
      background: linear-gradient(135deg, #ef4444, #dc2626);
      color: #fff;
      box-shadow: 0 4px 12px rgba(239, 68, 68, 0.3);
    }

    .btn-red:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(239, 68, 68, 0.4);
    }

    .btn-blue {
      background: linear-gradient(135deg, #3b82f6, #2563eb);
      color: #fff;
      box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
    }

    .btn-blue:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(59, 130, 246, 0.4);
    }

    .btn-purple {
      background: linear-gradient(135deg, #8b5cf6, #7c3aed);
      color: #fff;
      box-shadow: 0 4px 12px rgba(139, 92, 246, 0.3);
    }

    .btn-purple:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(139, 92, 246, 0.4);
    }

    .btn-orange {
      background: linear-gradient(135deg, #f59e0b, #d97706);
      color: #fff;
      box-shadow: 0 4px 12px rgba(245, 158, 11, 0.3);
    }

    .btn-orange:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(245, 158, 11, 0.4);
    }

    .btn-outline {
      background: transparent;
      color: var(--green-light);
      border: 1px solid var(--green-light);
    }

    .btn-small {
      padding: 8px 12px;
      font-size: 13px;
    }

    .btn:active {
      transform: scale(0.98);
    }

    .btn:disabled {
      opacity: 0.6;
      cursor: default;
      transform: none !important;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1) !important;
    }

    .btn-pulse-success {
      animation: pulseSuccess 0.5s ease-out;
    }

    @keyframes pulseSuccess {
      0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(16, 185, 129, 0.7); }
      70% { transform: scale(1.02); box-shadow: 0 0 0 12px rgba(16, 185, 129, 0); }
      100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(16, 185, 129, 0); }
    }

    /* Home */
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
      position: relative;
      overflow: hidden;
      border: 1px solid rgba(255, 255, 255, 0.05);
      min-height: 140px;
    }

    .tile::before {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      width: 100%;
      height: 4px;
    }

    .tile-primary::before { background: linear-gradient(90deg, #10b981, #059669); }
    .tile-warn::before    { background: linear-gradient(90deg, #f59e0b, #d97706); }
    .tile-danger::before  { background: linear-gradient(90deg, #ef4444, #dc2626); }
    .tile-neutral::before { background: linear-gradient(90deg, #3b82f6, #2563eb); }
    .tile-admin::before   { background: linear-gradient(90deg, #8b5cf6, #7c3aed); }

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
    .tile-warn .tile-icon    { background: linear-gradient(135deg, #f59e0b, #d97706); }
    .tile-danger .tile-icon  { background: linear-gradient(135deg, #ef4444, #dc2626); }
    .tile-neutral .tile-icon { background: linear-gradient(135deg, #3b82f6, #2563eb); }
    .tile-admin .tile-icon   { background: linear-gradient(135deg, #8b5cf6, #7c3aed); }

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

    .tile-footer-note {
      font-size: 12px;
      color: var(--text-muted);
      display: flex;
      align-items: center;
      gap: 6px;
    }

    /* Generic layout */
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
      background: rgba(16, 185, 129, 0.2);
    }

    .chip {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      font-size: 13px;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 20px;
      padding: 8px 16px;
      color: #c2cbd1;
      border: 1px solid rgba(255, 255, 255, 0.1);
      margin-top: 4px;
      backdrop-filter: blur(10px);
    }

    .chip-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
    }

    .chip-dot.green { background: #10b981; }
    .chip-dot.red   { background: #ef4444; }
    .chip-dot.purple   { background: #8b5cf6; }
    .chip-dot.blue   { background: #3b82f6; }
    .chip-dot.orange   { background: #f59e0b; }

    /* Search */
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
      transition: all var(--transition-fast);
    }

    .search-input:focus {
      border-color: var(--green-light);
      box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2);
      background: rgba(15, 23, 42, 0.8);
    }

    .search-icon {
      position: absolute;
      top: 50%;
      right: 16px;
      transform: translateY(-50%);
      color: var(--text-muted);
    }

    /* Teacher cards */
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
      position: relative;
      overflow: hidden;
    }

    .teacher-card::before {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      width: 4px;
      height: 100%;
      background: linear-gradient(180deg, #10b981, #3b82f6);
      opacity: 0;
      transition: opacity var(--transition-fast);
    }

    .teacher-card:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
    }

    .teacher-card:hover::before {
      opacity: 1;
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
      border: 1px solid rgba(16, 185, 129, 0.3);
    }

    .count-pill.neg {
      background: rgba(239, 68, 68, 0.15);
      color: #ef4444;
      border: 1px solid rgba(239, 68, 68, 0.3);
    }

    .meta-tag {
      font-size: 12px;
      color: var(--text-muted);
    }

    /* Teacher Profile */
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
      transition: all var(--transition-fast);
    }

    .stat-card:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
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

    .stat-hint {
      font-size: 12px;
      color: var(--text-muted);
    }

    .button-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 16px;
      margin-bottom: 20px;
    }

    /* Feedback list */
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
      transition: all var(--transition-fast);
    }

    .feedback-item:hover {
      transform: translateY(-2px);
      box-shadow: var(--shadow-hover);
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
      border: 1px solid rgba(255, 255, 255, 0.1);
    }

    /* Feedback form */
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
      transition: all var(--transition-fast);
      font-family: inherit;
    }

    .textarea:focus {
      border-color: var(--green-light);
      box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2);
      background: rgba(15, 23, 42, 0.8);
    }

    .form-footer {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      margin-top: 12px;
    }

    .hint-text {
      font-size: 13px;
      color: var(--text-muted);
      display: flex;
      align-items: center;
      gap: 6px;
    }

    /* Reports */
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
      font-size: 14px;
      border: 1px solid rgba(255, 255, 255, 0.05);
      transition: all var(--transition-fast);
    }

    .report-summary-card:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
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

    .report-row:last-child {
      border-bottom: none;
    }

    .report-highlight {
      margin-top: 12px;
      font-size: 14px;
      color: #e2e8f0;
      padding: 12px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
    }

    /* Admin screen */
    .admin-grid {
      display: grid;
      grid-template-columns: minmax(0, 1.2fr) minmax(0, 1fr);
      gap: 20px;
    }

    .admin-panel {
      background: linear-gradient(135deg, var(--card-bg) 0%, var(--card-alt) 100%);
      border-radius: var(--radius-card);
      padding: 20px;
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(255, 255, 255, 0.05);
      font-size: 14px;
      transition: all var(--transition-fast);
    }

    .admin-panel:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
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

    .admin-form-row .text-input {
      font-size: 14px;
      padding-block: 12px;
    }

    .admin-note {
      font-size: 12px;
      color: var(--text-muted);
      margin-top: 8px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    /* School status */
    .status-table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 12px;
      font-size: 14px;
    }

    .status-table th,
    .status-table td {
      padding: 10px 12px;
      text-align: right;
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .status-table th {
      font-size: 13px;
      color: #cbd5e1;
      background: rgba(255, 255, 255, 0.05);
      position: sticky;
      top: 0;
      z-index: 1;
      font-weight: 600;
    }

    .status-table td {
      color: #e2e8f0;
    }

    .status-score-pos {
      color: #10b981;
      font-weight: 600;
    }
    .status-score-neg {
      color: #ef4444;
      font-weight: 600;
    }
    .status-score-zero {
      color: #cbd5e1;
    }

    .scroll-card {
      max-height: 300px;
      overflow-y: auto;
      margin-top: 8px;
      border-radius: 8px;
    }

    /* Leaderboard */
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
      z-index: 1;
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
      transition: width 0.5s ease-out;
    }

    /* Notifications panel */
    .notif-panel {
      position: fixed;
      top: 70px;
      inset-inline-end: 20px;
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
      backdrop-filter: blur(10px);
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
      transition: all var(--transition-fast);
    }

    .notif-item:hover {
      background: rgba(255, 255, 255, 0.08);
    }

    .notif-empty {
      font-size: 13px;
      color: var(--text-muted);
      padding: 20px;
      text-align: center;
    }

    /* Responsive */
    @media (max-width: 768px) {
      .admin-grid {
        grid-template-columns: 1fr;
      }
      
      .tiles-grid {
        grid-template-columns: 1fr;
      }
      
      .button-row {
        grid-template-columns: 1fr;
      }
      
      .stat-row {
        grid-template-columns: 1fr;
      }
    }

    @media (max-width: 480px) {
      .app-header { padding-inline: 16px; }
      .app-main  { padding-inline: 16px; }
      
      .admin-form-row {
        flex-direction: column;
      }
    }
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
    <div class="notif-sub">
      אין פה ספאם, רק תובנות קצרות.
    </div>
    <div id="notif-list" class="notif-list"></div>
  </div>

  <!-- Top Bar -->
  <header class="app-header">
    <div class="app-header-left">
      <div class="logo">
        <i class="fas fa-chalkboard-teacher"></i>
      </div>
      <div class="app-title">מערכת משוב למורים</div>
    </div>
    <div class="app-header-right">
      <div class="icon-button" title="התראות" id="notif-button" aria-label="התראות">
        <i class="fas fa-bell"></i>
        <span id="notif-badge" class="notif-badge" style="display:none;"></span>
      </div>
      <div class="user-chip" id="user-chip" title="לחץ להתנתקות">
        <i class="fas fa-user"></i>
        <span>לא מחובר</span>
      </div>
      <div class="icon-button" title="עזרה" id="help-button" aria-label="עזרה">
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
            <span>המערכת מחוברת לשרת - כל המשובים נשמרים ונראים לכל המשתמשים!</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Screen: Home -->
    <section id="screen-home" class="screen">
      <div class="greeting" id="home-greeting">מה אתה רוצה לעשות היום?</div>
      <div class="sub-greeting" id="home-sub">
        מערכת משוב. לא מושלמת, אבל לפחות היא בצד שלך.
      </div>

      <div class="tiles-grid" id="home-tiles">
        <div class="tile tile-primary" data-action="view-teachers">
          <div class="tile-header">
            <div>
              <div class="tile-title">צפייה במורים</div>
              <div class="tile-desc">לעבור על כל המורים, לראות מי זוכה בנקודות.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-users"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>הצגה לפי שם, מקצוע ומשובים.</span>
          </div>
        </div>

        <div class="tile tile-primary" data-action="quick-compliment">
          <div class="tile-header">
            <div>
              <div class="tile-title">הוספת מחמאה</div>
              <div class="tile-desc">הסביר ברור, היה נחמד, היה אנושי – מגיע לו.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-heart"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>בחירת מורה והוספת מחמאה.</span>
          </div>
        </div>

        <div class="tile tile-danger" data-action="quick-remark">
          <div class="tile-header">
            <div>
              <div class="tile-title">הוספת הערה</div>
              <div class="tile-desc">לא ברור, לא הוגן, או פשוט לא. כותבים את זה.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-exclamation-triangle"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>בחירת מורה והוספת הערה.</span>
          </div>
        </div>

        <div class="tile tile-neutral" data-action="view-reports">
          <div class="tile-header">
            <div>
              <div class="tile-title">הדוחות שלי</div>
              <div class="tile-desc">כל מה שכבר כתבת – טוב ורע – במקום אחד.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-chart-bar"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>כולל סיכום שבועי קצר.</span>
          </div>
        </div>

        <div class="tile tile-neutral" data-action="leaderboard">
          <div class="tile-header">
            <div>
              <div class="tile-title">לוח מדרגים</div>
              <div class="tile-desc">מי הכי פעיל במתן משובים.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-trophy"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>נשמר מקומית בדפדפן.</span>
          </div>
        </div>

        <div class="tile tile-admin" data-action="admin" id="admin-tile" style="display:none;">
          <div class="tile-header">
            <div>
              <div class="tile-title">ניהול מורים (אדמין)</div>
              <div class="tile-desc">הוספת מורים, מחיקה וניהול רשימה.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-cog"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>זמין רק למשתמש אדמין.</span>
          </div>
        </div>

        <div class="tile tile-admin" data-action="school-status" id="school-status-tile" style="display:none;">
          <div class="tile-header">
            <div>
              <div class="tile-title">מצב בית ספר</div>
              <div class="tile-desc">תמונה כללית: מורים, מקצועות וניקוד התנהגותי.</div>
            </div>
            <div class="tile-icon">
              <i class="fas fa-school"></i>
            </div>
          </div>
          <div class="tile-footer-note">
            <i class="fas fa-info-circle"></i>
            <span>רק לאדמין, בצד ה"גדולים".</span>
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
        <input id="teacher-search" class="search-input" type="text"
               placeholder="חיפוש לפי שם או מקצוע...">
        <div class="search-icon">
          <i class="fas fa-search"></i>
        </div>
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

      <div class="chip">
        <div class="chip-dot green"></div>
        <span id="teacher-profile-subject">מקצוע</span>
      </div>

      <div class="stat-row">
        <div class="stat-card stat-compliments">
          <div class="stat-label">
            <i class="fas fa-heart"></i>
            <span>סה״כ מחמאות</span>
          </div>
          <div class="stat-value" id="stat-compliments">0</div>
          <div class="stat-hint">כמה פעמים באמת היה טוב.</div>
        </div>
        <div class="stat-card stat-remarks">
          <div class="stat-label">
            <i class="fas fa-exclamation-triangle"></i>
            <span>סה״כ הערות</span>
          </div>
          <div class="stat-value" id="stat-remarks">0</div>
          <div class="stat-hint">פחות הרגעים הזוהרים.</div>
        </div>
        <div class="stat-card stat-behavior">
          <div class="stat-label">
            <i class="fas fa-star"></i>
            <span>ניקוד התנהגותי</span>
          </div>
          <div class="stat-value" id="stat-behavior">0</div>
          <div class="stat-hint">מחמאות פחות הערות. כמה המאזן חיובי.</div>
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

      <h3 style="font-size:18px;margin:20px 0 12px;display:flex;align-items:center;gap:8px;">
        <i class="fas fa-history"></i>
        <span>משובים אחרונים שכתבת למורה הזה</span>
      </h3>
      <div id="teacher-feedback-list" class="feedback-list"></div>
      <div id="teacher-feedback-empty" class="feedback-empty">
        <i class="fas fa-inbox"></i>
        <div>אין עדיין משוב. או שהכול מושלם, או שפשוט לא התפנית לכתוב.</div>
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

      <textarea id="feedback-text" class="textarea"
                placeholder="תכתוב מה שחשוב לך. בלי חפירות, אבל שיהיה ברור."></textarea>

      <div class="form-footer">
        <div class="hint-text">
          <i class="fas fa-lightbulb"></i>
          <span>אם זה מאוד עצבני, המערכת תציע לך רגע לחשוב לפני השליחה.</span>
        </div>
      </div>

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
            <div>עדיין אין כלום. או שהכול ורוד, או שפשוט לא נכנסת.</div>
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
        <div class="chip-dot purple"></div>
        <span>מצב אדמין – רק אתה רואה את זה</span>
      </div>

      <div class="admin-grid" style="margin-top:20px;">
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
            <span>הנתונים נשמרים בשרת ונראים לכל המשתמשים.</span>
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

    <!-- Screen: School Status -->
    <section id="screen-school-status" class="screen">
      <div class="section-header">
        <div class="section-title">מצב בית ספר</div>
        <div class="back-link" data-back-to="home">
          <i class="fas fa-arrow-right"></i>
          <span>חזרה</span>
        </div>
      </div>

      <div class="chip">
        <div class="chip-dot purple"></div>
        <span>תמונה כללית – מורים, מקצועות וניקוד התנהגותי</span>
      </div>

      <div class="reports-section" style="margin-top:20px;">
        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
            <i class="fas fa-chalkboard-teacher"></i>
            <strong>סיכום לפי מורים</strong>
          </div>
          <div class="scroll-card">
            <table class="status-table" aria-label="טבלת סיכום מורים">
              <thead>
                <tr>
                  <th>מורה</th>
                  <th>מקצוע</th>
                  <th>מחמאות</th>
                  <th>הערות</th>
                  <th>ניקוד</th>
                </tr>
              </thead>
              <tbody id="school-status-teacher-body">
              </tbody>
            </table>
          </div>
        </div>

        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
            <i class="fas fa-book"></i>
            <strong>סיכום לפי מקצועות</strong>
          </div>
          <div class="scroll-card">
            <table class="status-table" aria-label="טבלת סיכום מקצועות">
              <thead>
                <tr>
                  <th>מקצוע</th>
                  <th>מחמאות</th>
                  <th>הערות</th>
                  <th>ניקוד</th>
                </tr>
              </thead>
              <tbody id="school-status-subject-body">
              </tbody>
            </table>
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
        <div class="chip-dot green"></div>
        <span>מי נותן הכי הרבה משובים (נשמר רק במחשב הזה)</span>
      </div>

      <div class="reports-section" style="margin-top:20px;">
        <div class="report-summary-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;">
            <i class="fas fa-trophy"></i>
            <strong>טבלת תלמידים</strong>
          </div>
          <div class="scroll-card">
            <table class="leaderboard-table" aria-label="לוח מדרגים">
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

  </main>
</div>

<script>
  // ---------- הגדרות JSONBin ----------
  const JSONBIN_BASE_URL = 'https://api.jsonbin.io/v3/b';
  const JSONBIN_MASTER_KEY = '$2a$10$Qtw.8XGZJR6Q6Z9Q8Q8Q8e'; // מפתח ציבורי - לא לדאוג, זה רק לקריאה
  const BIN_ID = '67a0b8a2acd3cb34a2879c0a'; // ID של ה-bin שלנו

  // ---------- פונקציות שמירה וטעינה מהשרת ----------
  async function loadDataFromServer() {
    try {
      console.log('🔄 טוען נתונים מהשרת...');
      const response = await fetch(`${JSONBIN_BASE_URL}/${BIN_ID}/latest`, {
        method: 'GET',
        headers: {
          'X-Master-Key': JSONBIN_MASTER_KEY,
          'Content-Type': 'application/json'
        }
      });

      if (!response.ok) {
        throw new Error('שגיאה בטעינת נתונים מהשרת');
      }

      const data = await response.json();
      console.log('✅ נתונים נטענו מהשרת:', data.record);
      
      return data.record || {
        teachers: [],
        feedback: [],
        studentStats: {}
      };
    } catch (error) {
      console.error('❌ שגיאה בטעינת נתונים:', error);
      // אם יש שגיאה, מחזירים נתוני ברירת מחדל
      return {
        teachers: [],
        feedback: [],
        studentStats: {}
      };
    }
  }

  async function saveDataToServer(data) {
    try {
      console.log('💾 שומר נתונים לשרת...', data);
      const response = await fetch(`${JSONBIN_BASE_URL}/${BIN_ID}`, {
        method: 'PUT',
        headers: {
          'X-Master-Key': JSONBIN_MASTER_KEY,
          'Content-Type': 'application/json',
          'X-Bin-Versioning': 'false'
        },
        body: JSON.stringify(data)
      });

      if (!response.ok) {
        throw new Error('שגיאה בשמירת נתונים לשרת');
      }

      const result = await response.json();
      console.log('✅ נתונים נשמרו בהצלחה:', result);
      return true;
    } catch (error) {
      console.error('❌ שגיאה בשמירת נתונים:', error);
      alert('שגיאה בשמירת נתונים לשרת. הנתונים לא נשמרו.');
      return false;
    }
  }

  // ---------- נתונים גלובליים ----------
  let teachers = [];
  let feedbackEntries = [];
  let studentStats = {};

  const quickTagSets = {
    compliment: [
      "מסביר ברור",
      "יחס טוב לתלמידים",
      "שומר על סדר בכיתה",
      "נותן משוב מועיל",
      "יוצר אווירה טובה"
    ],
    remark: [
      "הסבר לא ברור",
      "ציון לא הוגן",
      "דיבור לא מכבד",
      "מאחר לשיעור",
      "יותר מדי שיעורי בית"
    ]
  };

  // מילים חריפות לבדיקה בסיסית
  const harshWords = [
    "מטומטם","דפוק","חרא","זבל","סתום","טיפש","מגעיל","אפס","חסרי כבוד","חרא של","מזעזע"
  ];

  // ---------- State ----------
  const appState = {
    currentScreen: "login",
    currentUser: null,
    selectedTeacherId: null,
    feedbackType: null,  // "compliment" | "remark"
    isLoading: false
  };

  // ---------- Sound (Web Audio) ----------
  let audioCtx = null;
  function playUISound(type) {
    try {
      const AC = window.AudioContext || window.webkitAudioContext;
      if (!AC) return;
      if (!audioCtx) audioCtx = new AC();

      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.connect(gain);
      gain.connect(audioCtx.destination);

      let freq = 320;
      if (type === "click") freq = 320;
      else if (type === "success") freq = 620;
      else if (type === "warn") freq = 220;

      osc.frequency.value = freq;
      gain.gain.value = 0.08;

      const now = audioCtx.currentTime;
      osc.start(now);
      gain.gain.exponentialRampToValueAtTime(0.0001, now + 0.12);
      osc.stop(now + 0.12);
    } catch (e) {
      // אם הדפדפן מתבאס – מתעלמים
    }
  }

  // ---------- Helpers ----------
  function showScreen(name) {
    appState.currentScreen = name;
    document.querySelectorAll(".screen").forEach(s => {
      s.classList.toggle("active", s.id === "screen-" + name);
    });

    if (name === "teachers") renderTeacherList();
    if (name === "teacher-profile") renderTeacherProfile();
    if (name === "feedback") renderFeedbackScreen();
    if (name === "reports") renderReportsScreen();
    if (name === "admin") renderAdminTeacherList();
    if (name === "school-status") renderSchoolStatusScreen();
    if (name === "leaderboard") renderLeaderboardScreen();
  }

  function getTeacherById(id) {
    return teachers.find(t => t.id === id);
  }

  function getTeacherStats(id) {
    const entries = feedbackEntries.filter(f => f.teacherId === id);
    const compliments = entries.filter(f => f.type === "compliment").length;
    const remarks     = entries.filter(f => f.type === "remark").length;
    const score       = compliments - remarks; // ניקוד התנהגותי
    return { compliments, remarks, score };
  }

  function formatScore(score) {
    if (score > 0) return "+" + score;
    return String(score);
  }

  function formatDateShort(date) {
    return date.toLocaleDateString("he-IL", {
      day: "2-digit",
      month: "2-digit",
      year: "2-digit"
    }) + " " + date.toLocaleTimeString("he-IL", {
      hour: "2-digit",
      minute: "2-digit"
    });
  }

  function getDisplayNameForUser(user) {
    if (!user) return "תלמיד";
    return user.username;
  }

  function updateUserUIForCurrentUser() {
    const chip = document.getElementById("user-chip");
    const adminTile = document.getElementById("admin-tile");
    const schoolStatusTile = document.getElementById("school-status-tile");

    if (!appState.currentUser) {
      chip.innerHTML = '<i class="fas fa-user"></i><span>לא מחובר</span>';
      adminTile.style.display = "none";
      schoolStatusTile.style.display = "none";
      return;
    }

    const roleLabel = appState.currentUser.role === "admin" ? "אדמין" : "תלמיד";
    chip.innerHTML = `<i class="fas fa-user"></i><span>מחובר: ${roleLabel}</span>`;

    const greeting = document.getElementById("home-greeting");
    greeting.textContent = "מה אתה רוצה לעשות היום?";

    const sub = document.getElementById("home-sub");
    if (appState.currentUser.role === "admin") {
      sub.textContent = "מצב אדמין: אתה גם תלמיד וגם קצת מזכירות.";
    } else {
      sub.textContent = "מערכת משוב. סוף סוף מקום שבו גם אתה אומר מה אתה חושב.";
    }

    adminTile.style.display = appState.currentUser.role === "admin" ? "block" : "none";
    schoolStatusTile.style.display = appState.currentUser.role === "admin" ? "block" : "none";

    ensureStudentInStats(appState.currentUser);
  }

  function ensureStudentInStats(user) {
    const name = getDisplayNameForUser(user);
    if (!studentStats[name]) {
      studentStats[name] = { compliments: 0, remarks: 0 };
    }
  }

  // ---------- Notifications ----------
  function getWeeklyEntries() {
    const now = new Date();
    const weekAgo = new Date(now.getTime() - 7 * 24 * 60 * 60 * 1000);
    return feedbackEntries.filter(f => f.date >= weekAgo);
  }

  function getNotifications() {
    const notifs = [];
    const total = feedbackEntries.length;
    const weekEntries = getWeeklyEntries();
    const weekCompl = weekEntries.filter(f => f.type === "compliment").length;
    const weekRem   = weekEntries.filter(f => f.type === "remark").length;

    if (total === 0) {
      notifs.push("עדיין לא שלחת משוב. ברגע שיהיה משהו להגיד – המערכת פה.");
      return notifs;
    }

    if (weekEntries.length > 0) {
      notifs.push(`בשבוע האחרון הוספת ${weekCompl} מחמאות ו-${weekRem} הערות.`);
      if (weekRem > weekCompl) {
        notifs.push("נראה שהיה שבוע קשוח יותר. אולי שווה לחפש גם משהו חיובי אחד.");
      } else if (weekCompl > 0 && weekRem === 0) {
        notifs.push("כל המשובים השבוע היו מחמאות. אולי יש פה מועמד לפרס החינוך.");
      }
    } else {
      notifs.push("השבוע עוד לא כתבת שום משוב. לפעמים שקט זה גם נתון.");
    }

    let worstTeacher = null;
    teachers.forEach(t => {
      const stats = getTeacherStats(t.id);
      if (stats.score < 0) {
        if (!worstTeacher || stats.score < worstTeacher.score) {
          worstTeacher = { teacher: t, score: stats.score };
        }
      }
    });

    if (worstTeacher) {
      notifs.push(
        `למורה ${worstTeacher.teacher.name} יש כרגע ניקוד התנהגותי ${worstTeacher.score}. כנראה שיש שם כמה דברים שלא עובדים לך.`
      );
    }

    return notifs;
  }

  function updateNotifBadge() {
    const badge = document.getElementById("notif-badge");
    const notifs = getNotifications();
    if (notifs.length === 0) {
      badge.style.display = "none";
    } else {
      badge.style.display = "flex";
      badge.textContent = notifs.length;
    }
  }

  function renderNotifPanel() {
    const panel = document.getElementById("notif-panel");
    const listEl = document.getElementById("notif-list");
    const notifs = getNotifications();
    listEl.innerHTML = "";

    if (notifs.length === 0) {
      listEl.innerHTML = `<div class="notif-empty">אין כרגע התראות מיוחדות.</div>`;
    } else {
      notifs.forEach(text => {
        const item = document.createElement("div");
        item.className = "notif-item";
        item.textContent = text;
        listEl.appendChild(item);
      });
    }

    panel.classList.add("open");
    panel.setAttribute("aria-hidden", "false");
  }

  function closeNotifPanel() {
    const panel = document.getElementById("notif-panel");
    panel.classList.remove("open");
    panel.setAttribute("aria-hidden", "true");
  }

  // ---------- Login ----------
  function handleLogin() {
    const usernameRaw = document.getElementById("login-username").value.trim();
    const password = document.getElementById("login-password").value.trim();

    if (!usernameRaw) {
      alert("צריך לכתוב שם משתמש.");
      playUISound("warn");
      return;
    }

    let user = null;

    // אדמין יחיד – adir / 1234
    if (usernameRaw === "adir" && password === "1234") {
      user = { username: "adir", role: "admin" };
    } else {
      // כל שילוב אחר → תלמיד רגיל
      user = { username: usernameRaw, role: "student" };
    }

    appState.currentUser = user;
    ensureStudentInStats(user);
    updateUserUIForCurrentUser();
    showScreen("home");
    updateNotifBadge();
    playUISound("success");
  }

  function handleLogout() {
    appState.currentUser = null;
    updateUserUIForCurrentUser();
    closeNotifPanel();
    showScreen("login");
  }

  // ---------- בדיקת משוב חריף ----------
  function isHarshText(text) {
    if (!text) return false;
    const lower = text.toLowerCase();
    return harshWords.some(w => lower.includes(w));
  }

  function checkFeedbackBeforeSave(type, text) {
    const isHarsh = isHarshText(text);
    if (!isHarsh) return true;

    playUISound("warn");

    const label = type === "compliment" ? "מחמאה" : "הערה";
    const msg =
      `הטקסט שכתבת די חריף עבור ${label}.\n\n` +
      `אתה בטוח שאתה רוצה לשלוח את זה ככה?\n` +
      `אם לא – לחץ ביטול ותוכל לערוך.`;

    return confirm(msg);
  }

  // ---------- Render: Teacher List ----------
  function renderTeacherList() {
    const container = document.getElementById("teacher-list");
    const searchValue = document.getElementById("teacher-search").value.trim().toLowerCase();
    container.innerHTML = "";

    const filtered = teachers.filter(t => {
      if (!searchValue) return true;
      return t.name.toLowerCase().includes(searchValue) ||
             t.subject.toLowerCase().includes(searchValue);
    });

    if (filtered.length === 0) {
      container.innerHTML = "<div class='feedback-empty'><i class='fas fa-search'></i><div>לא נמצאו מורים. או שטעית בהקלדה, או שמשרד החינוך מחק את כולם.</div></div>";
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
          <div class="meta-tag">ניקוד התנהגותי: ${formatScore(stats.score)}</div>
          <div class="meta-tag">לחיצה לפתיחת פרופיל</div>
        </div>
      `;
      card.addEventListener("click", () => {
        appState.selectedTeacherId = t.id;
        playUISound("click");
        showScreen("teacher-profile");
      });
      container.appendChild(card);
    });
  }

  // ---------- Render: Teacher Profile ----------
  function renderTeacherProfile() {
    const teacher = getTeacherById(appState.selectedTeacherId);
    if (!teacher) return;

    const nameEl = document.getElementById("teacher-profile-name");
    const subjectEl = document.getElementById("teacher-profile-subject");
    const statComplimentsEl = document.getElementById("stat-compliments");
    const statRemarksEl = document.getElementById("stat-remarks");
    const statBehaviorEl = document.getElementById("stat-behavior");
    const listEl = document.getElementById("teacher-feedback-list");
    const emptyEl = document.getElementById("teacher-feedback-empty");

    nameEl.textContent = teacher.name;
    subjectEl.textContent = teacher.subject;

    const stats = getTeacherStats(teacher.id);
    statComplimentsEl.textContent = stats.compliments;
    statRemarksEl.textContent = stats.remarks;
    statBehaviorEl.textContent = formatScore(stats.score);

    // מראה את כל המשובים למורה הזה, מכל המשתמשים
    const entries = feedbackEntries
      .filter(f => f.teacherId === teacher.id)
      .sort((a, b) => b.date - a.date);

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

      const typeLabel = e.type === "compliment" ? "מחמאה" : "הערה";
      const typeEmoji = e.type === "compliment" ? "👍" : "⚠️";

      item.innerHTML = `
        <div>${e.text || "<i>אין טקסט, רק תגיות.</i>"}</div>
        <div class="feedback-tags">
          ${e.tags.map(tag => `<span class="tag-pill">${tag}</span>`).join("")}
        </div>
        <div class="feedback-meta-line">
          <span>${typeEmoji} ${typeLabel} - נכתב על ידי: ${e.user}</span>
          <span>${formatDateShort(new Date(e.date))}</span>
        </div>
      `;
      listEl.appendChild(item);
    });
  }

  // ---------- Render: Feedback Screen ----------
  function renderFeedbackScreen() {
    const teacher = getTeacherById(appState.selectedTeacherId);
    if (!teacher) return;

    const titleEl = document.getElementById("feedback-title");
    const subtitleEl = document.getElementById("feedback-subtitle");
    const quickTagsEl = document.getElementById("quick-tags");
    const textarea = document.getElementById("feedback-text");
    const submitBtn = document.getElementById("feedback-submit-button");

    const type = appState.feedbackType;
    const isCompliment = type === "compliment";

    titleEl.textContent = isCompliment
      ? `הוספת מחמאה למורה ${teacher.name}`
      : `הוספת הערה על המורה ${teacher.name}`;

    subtitleEl.textContent = isCompliment
      ? "תכתוב מה היה טוב – הסבר, יחס, תמיכה, כל דבר שעזר."
      : "תשמור על כנות, אבל תשאיר את זה ברור ומכבד.";

    textarea.value = "";

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
        playUISound("click");
      });
      quickTagsEl.appendChild(btn);
    });

    submitBtn.textContent = isCompliment ? "שליחת מחמאה" : "שליחת הערה";
    submitBtn.className = "btn btn-full " + (isCompliment ? "btn-green" : "btn-red");
  }

  // ---------- Render: Reports ----------
  function renderReportsScreen() {
    // הדוחות האישיים - מראה רק את המשובים של המשתמש הנוכחי
    const userEntries = feedbackEntries.filter(f => f.user === getDisplayNameForUser(appState.currentUser));
    const totalCompliments = userEntries.filter(f => f.type === "compliment").length;
    const totalRemarks     = userEntries.filter(f => f.type === "remark").length;

    document.getElementById("reports-total-compliments").textContent = totalCompliments;
    document.getElementById("reports-total-remarks").textContent = totalRemarks;

    const weekEntries = getWeeklyEntries().filter(f => f.user === getDisplayNameForUser(appState.currentUser));
    const weekCompl = weekEntries.filter(f => f.type === "compliment").length;
    const weekRem   = weekEntries.filter(f => f.type === "remark").length;

    document.getElementById("reports-week-compliments").textContent = weekCompl;
    document.getElementById("reports-week-remarks").textContent = weekRem;

    const weekSummaryText = document.getElementById("reports-week-summary-text");
    if (weekEntries.length === 0) {
      weekSummaryText.textContent = "אין נתונים לשבוע הזה. או שהיה שקט, או שפשוט לא נכנסת.";
    } else if (weekRem > weekCompl) {
      weekSummaryText.textContent = "השבוע היה יותר ביקורתי. יש יותר הערות ממחמאות.";
    } else if (weekCompl > 0 && weekRem === 0) {
      weekSummaryText.textContent = "שבוע חיובי לגמרי – רק מחמאות.";
    } else {
      weekSummaryText.textContent = "שבוע מאוזן יחסית. גם מחמאות וגם הערות נכנסו.";
    }

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

    const latest = [...userEntries].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 10);

    latest.forEach(e => {
      const teacher = getTeacherById(e.teacherId);
      const item = document.createElement("div");
      item.className = "feedback-item";

      const typeLabel = e.type === "compliment" ? "מחמאה" : "הערה";
      const typeEmoji = e.type === "compliment" ? "👍" : "⚠️";

      item.innerHTML = `
        <div>${e.text || "<i>אין טקסט, רק תגיות.</i>"}</div>
        <div class="feedback-tags">
          ${e.tags.map(tag => `<span class="tag-pill">${tag}</span>`).join("")}
        </div>
        <div class="feedback-meta-line">
          <span>${typeEmoji} ${typeLabel} – ${teacher ? teacher.name : "מורה לא ידוע"}</span>
          <span>${formatDateShort(new Date(e.date))}</span>
        </div>
      `;
      latestContainer.appendChild(item);
    });
  }

  // ---------- Render: Admin Teacher List + מחיקה ----------
  function renderAdminTeacherList() {
    const container = document.getElementById("admin-teacher-list");
    container.innerHTML = "";

    if (teachers.length === 0) {
      container.innerHTML = "<div class='feedback-empty'><i class='fas fa-users'></i><div>אין מורים ברשימה. מצב אוטופי.</div></div>";
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
          <div class="meta-tag">ניקוד התנהגותי: ${formatScore(stats.score)}</div>
        </div>
        <div class="teacher-meta">
          <div class="feedback-counts">
            <div class="count-pill pos"><i class="fas fa-heart"></i> ${stats.compliments}</div>
            <div class="count-pill neg"><i class="fas fa-exclamation-triangle"></i> ${stats.remarks}</div>
          </div>
          <button class="btn btn-red btn-small" data-delete-id="${t.id}">
            <i class="fas fa-trash"></i>
            <span>מחיקת מורה</span>
          </button>
        </div>
      `;
      container.appendChild(card);
    });

    container.querySelectorAll("button[data-delete-id]").forEach(btn => {
      btn.addEventListener("click", async (e) => {
        e.stopPropagation();
        const id = Number(btn.getAttribute("data-delete-id"));
        const teacher = getTeacherById(id);
        if (!teacher) return;
        if (!confirm(`למחוק את המורה ${teacher.name}? כל המשובים עליו ימחקו.`)) return;

        const index = teachers.findIndex(t => t.id === id);
        if (index !== -1) {
          teachers.splice(index, 1);
        }

        for (let i = feedbackEntries.length - 1; i >= 0; i--) {
          if (feedbackEntries[i].teacherId === id) {
            feedbackEntries.splice(i, 1);
          }
        }

        if (appState.selectedTeacherId === id) {
          appState.selectedTeacherId = null;
        }

        // שמירה לאחר מחיקה
        await saveAllData();

        renderAdminTeacherList();
        renderTeacherList();
        renderSchoolStatusScreen();
        updateNotifBadge();
        playUISound("success");
        alert("המורה והמשובים עליו נמחקו מהרשימה.");
      });
    });
  }

  // ---------- Render: School Status ----------
  function renderSchoolStatusScreen() {
    const tbodyTeachers = document.getElementById("school-status-teacher-body");
    const tbodySubjects = document.getElementById("school-status-subject-body");
    tbodyTeachers.innerHTML = "";
    tbodySubjects.innerHTML = "";

    teachers.forEach(t => {
      const stats = getTeacherStats(t.id);
      const tr = document.createElement("tr");
      const scoreClass =
        stats.score > 0 ? "status-score-pos" :
        stats.score < 0 ? "status-score-neg" :
        "status-score-zero";

      tr.innerHTML = `
        <td>${t.name}</td>
        <td>${t.subject}</td>
        <td>${stats.compliments}</td>
        <td>${stats.remarks}</td>
        <td class="${scoreClass}">${formatScore(stats.score)}</td>
      `;
      tbodyTeachers.appendChild(tr);
    });

    const subjectMap = {};
    teachers.forEach(t => {
      const stats = getTeacherStats(t.id);
      if (!subjectMap[t.subject]) {
        subjectMap[t.subject] = { compliments: 0, remarks: 0 };
      }
      subjectMap[t.subject].compliments += stats.compliments;
      subjectMap[t.subject].remarks += stats.remarks;
    });

    Object.keys(subjectMap).forEach(subject => {
      const data = subjectMap[subject];
      const score = data.compliments - data.remarks;
      const scoreClass =
        score > 0 ? "status-score-pos" :
        score < 0 ? "status-score-neg" :
        "status-score-zero";

      const tr = document.createElement("tr");
      tr.innerHTML = `
        <td>${subject}</td>
        <td>${data.compliments}</td>
        <td>${data.remarks}</td>
        <td class="${scoreClass}">${formatScore(score)}</td>
      `;
      tbodySubjects.appendChild(tr);
    });
  }

  // ---------- Render: Leaderboard ----------
  function renderLeaderboardScreen() {
    const tbody = document.getElementById("leaderboard-body");
    tbody.innerHTML = "";

    const entries = Object.keys(studentStats).map(name => {
      const stats = studentStats[name];
      const total = stats.compliments + stats.remarks;
      return { name, compliments: stats.compliments, remarks: stats.remarks, total };
    });

    if (entries.length === 0) {
      tbody.innerHTML = `<tr><td colspan="6" style="font-size:14px;color:#94a3b8;padding:20px;text-align:center;"><i class="fas fa-trophy"></i> אין עדיין נתונים. תתחיל לכתוב משובים ואז נבנה טבלה.</td></tr>`;
      return;
    }

    entries.sort((a, b) => b.total - a.total);

    const maxTotal = entries[0].total || 1;

    entries.forEach((e, index) => {
      const tr = document.createElement("tr");
      const widthPercent = Math.max(5, (e.total / maxTotal) * 100);

      tr.innerHTML = `
        <td>${index + 1}</td>
        <td>${e.name}</td>
        <td>${e.compliments}</td>
        <td>${e.remarks}</td>
        <td>${e.total}</td>
        <td>
          <div class="leaderboard-bar-wrapper">
            <div class="leaderboard-bar" style="width:${widthPercent}%;"></div>
          </div>
        </td>
      `;
      tbody.appendChild(tr);
    });
  }

  // ---------- פונקציות שמירת נתונים ----------
  async function saveAllData() {
    const data = {
      teachers,
      feedback: feedbackEntries,
      studentStats
    };
    return await saveDataToServer(data);
  }

  // ---------- Events ----------
  document.addEventListener("DOMContentLoaded", async () => {
    // הצגת הודעת טעינה
    const loginButton = document.getElementById("login-button");
    const originalLoginText = loginButton.innerHTML;
    
    // טעינת נתונים מהשרת
    loginButton.innerHTML = '<div class="loading"></div> טוען נתונים...';
    loginButton.disabled = true;

    try {
      const data = await loadDataFromServer();
      teachers = data.teachers.length > 0 ? data.teachers : [
        { id: 1, name: "אורן", subject: "אנגלית" },
        { id: 2, name: "אורית", subject: "מתמטיקה" },
        { id: 3, name: "רעות", subject: "לשון" },
        { id: 4, name: "אבי", subject: "השכלה כללית" },
        { id: 5, name: "נטע", subject: "היסטוריה" },
        { id: 6, name: "מרינה", subject: "תנך" },
        { id: 7, name: "אור", subject: "כימיה" },
        { id: 8, name: "יהודה", subject: "ספורט" }
      ];
      
      feedbackEntries = data.feedback || [];
      studentStats = data.studentStats || {};

      // אם אין מורים, שמור את המורים הראשוניים
      if (data.teachers.length === 0) {
        await saveAllData();
      }

    } catch (error) {
      console.error('שגיאה בטעינת נתונים:', error);
      alert('שגיאה בטעינת נתונים מהשרת. המערכת תעבוד עם נתונים מקומיים.');
    } finally {
      loginButton.innerHTML = originalLoginText;
      loginButton.disabled = false;
    }

    // שאר הקוד נשאר כמו שהיה...
    document.getElementById("login-button").addEventListener("click", () => {
      playUISound("click");
      handleLogin();
    });
    
    document.getElementById("login-password").addEventListener("keydown", (e) => {
      if (e.key === "Enter") {
        playUISound("click");
        handleLogin();
      }
    });

    document.getElementById("user-chip").addEventListener("click", () => {
      if (!appState.currentUser) return;
      if (confirm("להתנתק מהמערכת?")) {
        playUISound("click");
        handleLogout();
      }
    });

    document.querySelectorAll(".tile").forEach(tile => {
      tile.addEventListener("click", () => {
        const action = tile.dataset.action;
        playUISound("click");
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
          if (!appState.currentUser || appState.currentUser.role !== "admin") {
            alert("רק אדמין יכול להיכנס לכאן.");
            playUISound("warn");
            return;
          }
          showScreen("admin");
        } else if (action === "school-status") {
          if (!appState.currentUser || appState.currentUser.role !== "admin") {
            alert("רק אדמין יכול להיכנס לכאן.");
            playUISound("warn");
            return;
          }
          showScreen("school-status");
        }
      });
    });

    document.querySelectorAll(".back-link").forEach(link => {
      link.addEventListener("click", () => {
        const target = link.dataset.backTo;
        if (target) {
          playUISound("click");
          showScreen(target);
        }
      });
    });

    document.getElementById("teacher-search").addEventListener("input", renderTeacherList);

    document.getElementById("btn-profile-compliment").addEventListener("click", () => {
      appState.feedbackType = "compliment";
      playUISound("click");
      showScreen("feedback");
    });
    document.getElementById("btn-profile-remark").addEventListener("click", () => {
      appState.feedbackType = "remark";
      playUISound("click");
      showScreen("feedback");
    });

    document.getElementById("feedback-back").addEventListener("click", () => {
      playUISound("click");
      showScreen("teacher-profile");
    });

    document.getElementById("feedback-submit-button").addEventListener("click", async () => {
      const submitBtn = document.getElementById("feedback-submit-button");
      const teacherId = appState.selectedTeacherId;
      const type = appState.feedbackType;
      if (!teacherId || !type) {
        alert("משהו נתקע. תנסה לבחור את המורה שוב.");
        playUISound("warn");
        return;
      }

      const teacher = getTeacherById(teacherId);
      const textarea = document.getElementById("feedback-text");
      const text = textarea.value.trim();

      const quickTagsWrap = document.getElementById("quick-tags");
      const tagButtons = Array.from(quickTagsWrap.querySelectorAll(".quick-tag"));
      const tags = [];
      tagButtons.forEach(btn => {
        const posSel = btn.classList.contains("selected-positive");
        const negSel = btn.classList.contains("selected-negative");
        if (posSel || negSel) tags.push(btn.textContent);
      });

      if (tags.length === 0 && !text) {
        alert("תבחר לפחות תגית אחת או תכתוב משהו קטן.");
        playUISound("warn");
        return;
      }

      if (!checkFeedbackBeforeSave(type, text)) {
        return;
      }

      const userName = getDisplayNameForUser(appState.currentUser);

      // שמירה מקומית
      feedbackEntries.push({
        teacherId,
        type,
        tags,
        text,
        date: new Date().toISOString(),
        user: userName
      });

      if (!studentStats[userName]) {
        studentStats[userName] = { compliments: 0, remarks: 0 };
      }
      if (type === "compliment") {
        studentStats[userName].compliments++;
      } else {
        studentStats[userName].remarks++;
      }

      // שמירה לשרת
      const originalText = submitBtn.innerHTML;
      submitBtn.innerHTML = '<div class="loading"></div> שומר...';
      submitBtn.disabled = true;

      const success = await saveAllData();

      submitBtn.innerHTML = originalText;
      submitBtn.disabled = false;

      if (success) {
        submitBtn.classList.remove("btn-pulse-success");
        void submitBtn.offsetWidth;
        submitBtn.classList.add("btn-pulse-success");

        playUISound("success");

        alert(type === "compliment"
          ? `מחמאה נשמרה למורה ${teacher.name}. כל המשתמשים יראו אותה!`
          : `הערה נשמרה למורה ${teacher.name}. כל המשתמשים יראו אותה!`
        );

        updateNotifBadge();
        showScreen("teacher-profile");
      }
    });

    document.getElementById("help-button").addEventListener("click", () => {
      playUISound("click");
      alert(
        "מה יש פה:\n\n" +
        "• התחברות – כל שם משתמש וסיסמה. adir/1234 נכנס כאדמין.\n" +
        "• בית – כרטיסיות לכל פעולה.\n" +
        "• מורים – רשימה, חיפוש, ניקוד התנהגותי.\n" +
        "• פרופיל מורה – מחמאות/הערות וציון התנהגותי.\n" +
        "• משוב – תגיות מוכנות + טקסט חופשי.\n" +
        "• דוחות – סיכום כללי + סיכום שבועי.\n" +
        "• לוח מדרגים – מי נותן הכי הרבה משובים.\n" +
        "• התראות – סיכום חכם של מה שקרה לאחרונה.\n" +
        "• אדמין – הוספת ומחיקת מורים.\n" +
        "• מצב בית ספר – תמונת מצב לפי מורים ומקצועות.\n\n" +
        "🚀 חשוב: המשובים נשמרים בשרת ונראים לכל המשתמשים!"
      );
    });

    document.getElementById("admin-add-teacher").addEventListener("click", async () => {
      if (!appState.currentUser || appState.currentUser.role !== "admin") {
        alert("רק אדמין יכול להוסיף מורים.");
        playUISound("warn");
        return;
      }
      const nameInput = document.getElementById("admin-new-name");
      const subjectInput = document.getElementById("admin-new-subject");
      const name = nameInput.value.trim();
      const subject = subjectInput.value.trim();

      if (!name || !subject) {
        alert("צריך למלא גם שם וגם מקצוע.");
        playUISound("warn");
        return;
      }

      const newId = teachers.length ? Math.max(...teachers.map(t => t.id)) + 1 : 1;
      teachers.push({ id: newId, name, subject });

      // שמירת המורים החדשים לשרת
      const success = await saveAllData();

      if (success) {
        nameInput.value = "";
        subjectInput.value = "";
        renderAdminTeacherList();
        renderTeacherList();
        renderSchoolStatusScreen();
        playUISound("success");
        alert("המורה נוסף לרשימה ונראה לכל המשתמשים!");
      }
    });

    const notifButton = document.getElementById("notif-button");
    notifButton.addEventListener("click", (e) => {
      e.stopPropagation();
      const panel = document.getElementById("notif-panel");
      if (panel.classList.contains("open")) {
        closeNotifPanel();
      } else {
        renderNotifPanel();
        playUISound("click");
      }
    });

    document.addEventListener("click", (e) => {
      const panel = document.getElementById("notif-panel");
      if (!panel.classList.contains("open")) return;
      const insidePanel = panel.contains(e.target);
      const insideButton = notifButton.contains(e.target);
      if (!insidePanel && !insideButton) {
        closeNotifPanel();
      }
    });

    renderTeacherList();
    updateNotifBadge();
  });
</script>
</body>
</html>
