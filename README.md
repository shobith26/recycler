<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>recycling-hub-final</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://api.fontshare.com/v2/css?f[]=general-sans@400,500,600,700&f[]=clash-display@500,600,700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

  <style>
    :root, [data-theme="light"] {
      --text-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
      --text-sm: clamp(0.875rem, 0.8rem + 0.35vw, 1rem);
      --text-base: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);
      --text-lg: clamp(1.125rem, 1rem + 0.75vw, 1.5rem);
      --text-xl: clamp(1.5rem, 1.2rem + 1.25vw, 2.25rem);
      --text-2xl: clamp(2rem, 1.2rem + 2.5vw, 3.5rem);

      --space-2: 0.5rem;
      --space-3: 0.75rem;
      --space-4: 1rem;
      --space-6: 1.5rem;
      --space-8: 2rem;
      --space-10: 2.5rem;

      --color-bg: #f7f6f2;
      --color-surface: #fcfbf8;
      --color-surface-2: #f1efe8;
      --color-border: #d6d1c7;
      --color-divider: #e2ddd3;
      --color-text: #211f19;
      --color-text-muted: #6f6b63;
      --color-text-inverse: #f8f7f3;
      --color-primary: #056c67;
      --color-primary-hover: #04514d;
      --color-primary-highlight: #d7ece9;
      --color-success: #3d7a21;
      --color-warning: #b86a1a;
      --color-error: #b2316c;
      --color-blue: #0f6ea8;

      --radius-lg: 1.1rem;
      --radius-xl: 1.5rem;
      --radius-full: 9999px;

      --shadow-sm: 0 1px 3px rgba(24, 27, 22, 0.08);
      --shadow-md: 0 10px 24px rgba(24, 27, 22, 0.09);

      --font-body: 'General Sans', 'Inter', sans-serif;
      --font-display: 'Clash Display', 'General Sans', sans-serif;
      --transition: 180ms cubic-bezier(0.16, 1, 0.3, 1);
    }

    [data-theme="dark"] {
      --color-bg: #141513;
      --color-surface: #1b1d1a;
      --color-surface-2: #232622;
      --color-border: #373b36;
      --color-divider: #30342f;
      --color-text: #ece9e2;
      --color-text-muted: #b4b0a7;
      --color-text-inverse: #10110f;
      --color-primary: #64b5ac;
      --color-primary-hover: #7cc7be;
      --color-primary-highlight: #1f3633;
      --color-success: #87c765;
      --color-warning: #f0a14a;
      --color-error: #e46aa0;
      --color-blue: #79b7e6;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: var(--font-body);
      font-size: var(--text-base);
      color: var(--color-text);
      background:
        radial-gradient(circle at top right, color-mix(in srgb, var(--color-primary) 12%, transparent), transparent 28%),
        radial-gradient(circle at left bottom, color-mix(in srgb, var(--color-blue) 10%, transparent), transparent 22%),
        var(--color-bg);
      min-height: 100vh;
    }

    button, input, textarea, select { font: inherit; color: inherit; }
    button { cursor: pointer; border: none; background: none; }

    .hidden { display: none !important; }
    .small { font-size: var(--text-sm); }
    .tiny { font-size: var(--text-xs); }
    .muted { color: var(--color-text-muted); }

    .card {
      background: color-mix(in srgb, var(--color-surface) 92%, transparent);
      border: 1px solid color-mix(in srgb, var(--color-text) 10%, transparent);
      border-radius: var(--radius-xl);
      box-shadow: var(--shadow-sm);
      backdrop-filter: blur(10px);
    }

    .btn {
      min-height: 44px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: .5rem;
      padding: .9rem 1.2rem;
      border-radius: var(--radius-full);
      transition: all var(--transition);
      font-weight: 600;
    }

    .btn-primary {
      background: var(--color-primary);
      color: var(--color-text-inverse);
      box-shadow: var(--shadow-md);
    }

    .btn-secondary {
      background: var(--color-surface);
      border: 1px solid color-mix(in srgb, var(--color-text) 10%, transparent);
    }

    .btn-danger {
      background: color-mix(in srgb, var(--color-error) 14%, var(--color-surface));
      color: var(--color-error);
      border: 1px solid color-mix(in srgb, var(--color-error) 22%, transparent);
    }

    .pill {
      display: inline-flex;
      align-items: center;
      border-radius: var(--radius-full);
      padding: .55rem .9rem;
      background: var(--color-primary-highlight);
      color: var(--color-primary);
      font-size: var(--text-xs);
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: .05em;
    }

    .login-shell {
      min-height: 100vh;
      display: grid;
      place-items: center;
      padding: var(--space-6);
    }

    .login-wrap {
      width: min(1120px, 100%);
      display: grid;
      grid-template-columns: 1.05fr .95fr;
      overflow: hidden;
    }

    .brand-panel {
      padding: clamp(2rem, 5vw, 4rem);
      background: linear-gradient(145deg, color-mix(in srgb, var(--color-primary) 92%, black 8%), color-mix(in srgb, var(--color-blue) 35%, var(--color-primary) 65%));
      color: white;
    }

    .brand-logo {
      display: inline-flex;
      align-items: center;
      gap: .9rem;
      margin-bottom: var(--space-10);
      font-size: 1.1rem;
      font-weight: 700;
    }

    .brand-logo svg { width: 42px; height: 42px; }

    .brand-panel h1 {
      font-family: var(--font-display);
      font-size: var(--text-2xl);
      line-height: 1.05;
      max-width: 10ch;
      margin-bottom: 1rem;
    }

    .impact-list {
      margin-top: var(--space-10);
      display: grid;
      gap: 1rem;
    }

    .impact-item {
      padding: 1rem 1.1rem;
      border-radius: var(--radius-lg);
      background: rgba(255,255,255,0.08);
      border: 1px solid rgba(255,255,255,0.12);
    }

    .login-panel {
      padding: clamp(2rem, 5vw, 4rem);
      background: var(--color-surface);
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    .login-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 1.5rem;
    }

    .theme-toggle {
      width: 46px;
      height: 46px;
      display: grid;
      place-items: center;
      border-radius: 50%;
      background: var(--color-surface-2);
      border: 1px solid color-mix(in srgb, var(--color-text) 10%, transparent);
    }

    .role-switch {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: .75rem;
      margin: 1rem 0;
    }

    .role-btn {
      padding: .95rem 1rem;
      border-radius: var(--radius-full);
      background: var(--color-surface-2);
      border: 1px solid transparent;
      font-weight: 600;
    }

    .role-btn.active {
      background: var(--color-primary-highlight);
      color: var(--color-primary);
      border-color: color-mix(in srgb, var(--color-primary) 25%, transparent);
    }

    .form-grid { display: grid; gap: 1rem; }
    .field { display: grid; gap: .45rem; }

    .field input,
    .field select,
    .field textarea {
      width: 100%;
      min-height: 48px;
      border-radius: var(--radius-lg);
      border: 1px solid var(--color-border);
      background: var(--color-bg);
      padding: .95rem 1rem;
    }

    .field textarea { min-height: 110px; resize: vertical; }

    .error-box, .success-box, .note-box {
      display: none;
      margin-top: 1rem;
      padding: .9rem 1rem;
      border-radius: var(--radius-lg);
      font-size: var(--text-sm);
    }

    .error-box.show, .success-box.show, .note-box.show { display: block; }

    .error-box {
      background: color-mix(in srgb, var(--color-error) 10%, var(--color-surface));
      color: var(--color-error);
      border: 1px solid color-mix(in srgb, var(--color-error) 22%, transparent);
    }

    .success-box {
      background: color-mix(in srgb, var(--color-success) 12%, var(--color-surface));
      color: var(--color-success);
      border: 1px solid color-mix(in srgb, var(--color-success) 22%, transparent);
    }

    .note-box {
      background: color-mix(in srgb, var(--color-blue) 12%, var(--color-surface));
      color: var(--color-blue);
      border: 1px solid color-mix(in srgb, var(--color-blue) 22%, transparent);
    }

    .app-shell {
      min-height: 100vh;
      display: grid;
      grid-template-columns: 280px 1fr;
    }

    .sidebar {
      background: color-mix(in srgb, var(--color-surface) 88%, transparent);
      border-right: 1px solid color-mix(in srgb, var(--color-text) 10%, transparent);
      padding: 1.2rem;
      height: 100vh;
      position: sticky;
      top: 0;
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

    .brand-mini {
      display: flex;
      gap: .8rem;
      align-items: center;
      padding: .75rem;
      border-radius: var(--radius-lg);
      background: var(--color-surface-2);
      border: 1px solid color-mix(in srgb, var(--color-text) 9%, transparent);
    }

    .brand-mini svg { width: 38px; height: 38px; color: var(--color-primary); }

    .nav-list {
      display: grid;
      gap: .45rem;
      list-style: none;
    }

    .nav-btn {
      width: 100%;
      text-align: left;
      min-height: 46px;
      padding: .95rem 1rem;
      border-radius: var(--radius-lg);
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid transparent;
    }

    .nav-btn.active,
    .nav-btn:hover {
      background: var(--color-surface-2);
      border-color: color-mix(in srgb, var(--color-text) 10%, transparent);
    }

    .sidebar-footer {
      margin-top: auto;
      padding: 1rem;
      border-radius: var(--radius-lg);
      background: var(--color-surface-2);
      border: 1px solid color-mix(in srgb, var(--color-text) 9%, transparent);
    }

    .main { min-width: 0; padding: 1.2rem; }

    .topbar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 1rem;
      margin-bottom: 1.2rem;
      padding: 1rem 1.1rem;
    }

    .topbar h1 {
      font-family: var(--font-display);
      font-size: clamp(1.6rem, 2vw, 2.5rem);
      line-height: 1.08;
      margin-bottom: .25rem;
    }

    .topbar-actions {
      display: flex;
      gap: .8rem;
      flex-wrap: wrap;
    }

    .grid-stats {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1rem;
      margin-bottom: 1rem;
    }

    .stat-card, .panel {
      padding: 1.2rem;
      border-radius: var(--radius-xl);
    }

    .stat-card h3 {
      font-size: var(--text-sm);
      color: var(--color-text-muted);
      margin-bottom: .45rem;
    }

    .stat-value {
      font-family: var(--font-display);
      font-size: clamp(1.5rem, 2vw, 2.2rem);
      line-height: 1;
      margin-bottom: .4rem;
    }

    .dashboard-grid {
      display: grid;
      grid-template-columns: 1.15fr .85fr;
      gap: 1rem;
    }

    .panel-header {
      display: flex;
      justify-content: space-between;
      align-items: start;
      gap: 1rem;
      margin-bottom: 1rem;
    }

    .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
    .price-grid, .timeline, .notif-list, .chat-list, .history-list, .scanner-results {
      display: grid;
      gap: 1rem;
    }

    .price-grid { grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); }

    .pickup-summary,
    .timeline-item,
    .notif-item,
    .chat-item,
    .history-item,
    .scan-item,
    .kpi-mini {
      padding: 1rem;
      border-radius: var(--radius-lg);
      background: var(--color-surface-2);
      border: 1px solid color-mix(in srgb, var(--color-text) 8%, transparent);
    }

    .status-badge {
      display: inline-flex;
      align-items: center;
      gap: .45rem;
      padding: .45rem .7rem;
      border-radius: var(--radius-full);
      font-size: var(--text-xs);
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: .04em;
    }

    .status-pending {
      background: color-mix(in srgb, var(--color-warning) 14%, var(--color-surface));
      color: var(--color-warning);
    }

    .status-accepted {
      background: color-mix(in srgb, var(--color-blue) 14%, var(--color-surface));
      color: var(--color-blue);
    }

    .status-completed {
      background: color-mix(in srgb, var(--color-success) 14%, var(--color-surface));
      color: var(--color-success);
    }

    .split-actions {
      display: flex;
      flex-wrap: wrap;
      gap: .75rem;
      margin-top: 1rem;
    }

    .view { display: none; }
    .view.active { display: block; }

    #map {
      width: 100%;
      min-height: 360px;
      border-radius: var(--radius-xl);
      overflow: hidden;
      border: 1px solid color-mix(in srgb, var(--color-text) 8%, transparent);
    }

    .scanner-drop {
      border: 1.5px dashed color-mix(in srgb, var(--color-primary) 28%, transparent);
      background: color-mix(in srgb, var(--color-primary) 6%, var(--color-surface));
      border-radius: var(--radius-xl);
      padding: 1.2rem;
      text-align: center;
    }

    .preview-box {
      display: grid;
      place-items: center;
      border-radius: var(--radius-xl);
      min-height: 220px;
      overflow: hidden;
      background: var(--color-surface-2);
      border: 1px solid color-mix(in srgb, var(--color-text) 8%, transparent);
      margin-top: 1rem;
    }

    .preview-box img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .chat-compose {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: .75rem;
      margin-top: 1rem;
    }

    .mobile-bar {
      display: none;
      position: sticky;
      bottom: 0;
      z-index: 40;
      margin-top: 1rem;
      padding: .7rem;
      background: color-mix(in srgb, var(--color-surface) 92%, transparent);
      border: 1px solid color-mix(in srgb, var(--color-text) 8%, transparent);
      border-radius: var(--radius-xl);
    }

    .mobile-nav {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: .5rem;
    }

    .mobile-nav button {
      min-height: 46px;
      border-radius: var(--radius-lg);
      background: var(--color-surface-2);
      font-size: var(--text-xs);
      font-weight: 700;
    }

    @media (max-width: 1100px) {
      .grid-stats { grid-template-columns: repeat(2, 1fr); }
      .dashboard-grid, .two-col, .login-wrap { grid-template-columns: 1fr; }
    }

    @media (max-width: 860px) {
      .app-shell { grid-template-columns: 1fr; }
      .sidebar { display: none; }
      .mobile-bar { display: block; }
      .main { padding-bottom: 6rem; }
      .topbar { flex-direction: column; align-items: flex-start; }
    }

    @media (max-width: 600px) {
      .grid-stats, .price-grid { grid-template-columns: 1fr; }
      .role-switch { grid-template-columns: 1fr; }
      .chat-compose { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <section id="loginPage" class="login-shell">
    <div class="login-wrap card">
      <div class="brand-panel">
        <div class="brand-logo">
          <svg viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M31 6l8 14h-8l-4 7-8-14L31 6Z" fill="currentColor"/>
            <path d="M15 34l8-13 4 7h8l-8 13H15Z" fill="currentColor" opacity=".88"/>
            <path d="M49 34l-8 13-4-7h-8l8-13h12Z" fill="currentColor" opacity=".72"/>
          </svg>
          <span>Recycling Hub</span>
        </div>

        <span class="pill">Final Role-Based UI</span>
        <h1>Smart recycling platform for users and recyclers.</h1>
        <p>User and recycler dashboards now show separate role-specific summary cards.</p>

        <div class="impact-list">
          <div class="impact-item">User dashboard shows request-focused stats only.</div>
          <div class="impact-item">Recycler dashboard shows handling-focused stats only.</div>
          <div class="impact-item">Notifications and recycler history can now be cleared instantly.</div>
        </div>
      </div>

      <div class="login-panel">
        <div class="login-header">
          <div>
            <h2 style="font-family:var(--font-display); font-size:var(--text-xl);">Sign in</h2>
            <p class="muted small">Choose a role and continue.</p>
          </div>
          <button class="theme-toggle" id="themeToggle" type="button">🌙</button>
        </div>

        <div class="role-switch">
          <button class="role-btn active" id="userRoleBtn" type="button">User Login</button>
          <button class="role-btn" id="recyclerRoleBtn" type="button">Recycler Login</button>
        </div>

        <form id="loginForm" class="form-grid">
          <div class="field">
            <label for="loginName">Full name</label>
            <input id="loginName" type="text" placeholder="Enter full name" required />
          </div>

          <div class="field">
            <label for="loginPhone">Phone number</label>
            <input id="loginPhone" type="tel" placeholder="Enter phone number" required />
          </div>

          <div class="field hidden" id="secretFieldWrap">
            <label for="secretCode">Recycler secret code</label>
            <input id="secretCode" type="password" placeholder="Enter recycler secret code" />
          </div>

          <button class="btn btn-primary" type="submit">Enter Dashboard</button>
        </form>

        <div id="loginError" class="error-box"></div>
        <div id="loginSuccess" class="success-box"></div>
      </div>
    </div>
  </section>

  <section id="appPage" class="app-shell hidden">
    <aside class="sidebar">
      <div class="brand-mini">
        <svg viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M31 6l8 14h-8l-4 7-8-14L31 6Z" fill="currentColor"/>
          <path d="M15 34l8-13 4 7h8l-8 13H15Z" fill="currentColor" opacity=".88"/>
          <path d="M49 34l-8 13-4-7h-8l8-13h12Z" fill="currentColor" opacity=".72"/>
        </svg>
        <div>
          <h3>Recycling Hub</h3>
          <p class="tiny muted" id="roleLabel">User dashboard</p>
        </div>
      </div>

      <nav>
        <ul class="nav-list">
          <li><button class="nav-btn active" data-view="dashboard">Dashboard <span>→</span></button></li>
          <li><button class="nav-btn" data-view="pickup">Pickup Flow <span>→</span></button></li>
          <li><button class="nav-btn" data-view="pricing">Price Board <span>→</span></button></li>
          <li><button class="nav-btn" data-view="mapView">User Location <span>→</span></button></li>
          <li><button class="nav-btn user-only" data-view="scanner">Waste Scanner <span>→</span></button></li>
          <li><button class="nav-btn" data-view="notifications">Notifications <span>→</span></button></li>
          <li><button class="nav-btn" data-view="chat">Messages <span>→</span></button></li>
          <li><button class="nav-btn recycler-only" data-view="history">Recycler History <span>→</span></button></li>
        </ul>
      </nav>

      <div class="sidebar-footer">
        <p class="small"><strong id="welcomeName">Welcome</strong></p>
        <p class="tiny muted" id="welcomeMeta">Manage your recycling workflow.</p>
        <div class="split-actions">
          <button id="sidebarThemeBtn" class="btn btn-secondary" type="button">Theme</button>
          <button id="logoutBtn" class="btn btn-danger" type="button">Logout</button>
        </div>
      </div>
    </aside>

    <main class="main">
      <header class="topbar card">
        <div>
          <span class="pill" id="rolePill">User Portal</span>
          <h1 id="pageTitle">Dashboard</h1>
          <p class="muted" id="pageSubtitle">Role-based overview of current recycling activity.</p>
        </div>
        <div class="topbar-actions">
          <button id="topThemeBtn" class="btn btn-secondary" type="button">Toggle Theme</button>
          <button id="topLogoutBtn" class="btn btn-danger" type="button">Logout</button>
        </div>
      </header>

      <section id="view-dashboard" class="view active">
        <div class="grid-stats" id="dashboardStats"></div>

        <div class="dashboard-grid">
          <article class="panel card">
            <div class="panel-header">
              <div>
                <h2 style="font-family:var(--font-display);">Live overview</h2>
                <p class="muted small" id="overviewSubtitle">Latest request and current status.</p>
              </div>
              <span id="statusBadge" class="status-badge status-pending">No active booking</span>
            </div>
            <div id="latestPickupCard" class="pickup-summary">
              <p class="muted">No active pickup yet.</p>
            </div>
          </article>

          <article class="panel card">
            <div class="panel-header">
              <div>
                <h2 style="font-family:var(--font-display);">Quick summary</h2>
                <p class="muted small" id="quickSummarySubtitle">Role-based dashboard summary.</p>
              </div>
            </div>
            <div class="timeline" id="dashboardQuickCards"></div>
          </article>
        </div>
      </section>

      <section id="view-pickup" class="view">
        <div class="two-col">
          <article class="panel card user-only">
            <div class="panel-header">
              <div>
                <h2 style="font-family:var(--font-display);">Book pickup</h2>
                <p class="muted small">User booking form remains unchanged.</p>
              </div>
            </div>

            <form id="pickupForm" class="form-grid">
              <div class="field">
                <label for="wasteType">Waste type</label>
                <select id="wasteType" required>
                  <option value="">Select material</option>
                  <option value="plastic">Plastic</option>
                  <option value="paper">Paper</option>
                  <option value="metal">Metal</option>
                  <option value="glass">Glass</option>
                  <option value="ewaste">E-Waste</option>
                </select>
              </div>

              <div class="field">
                <label for="wasteWeight">Weight (kg)</label>
                <input id="wasteWeight" type="number" min="1" step="0.1" required />
              </div>

              <div class="field">
                <label for="pickupDate">Pickup date</label>
                <input id="pickupDate" type="date" required />
              </div>

              <div class="field">
                <label for="pickupAddress">Pickup address</label>
                <textarea id="pickupAddress" required></textarea>
              </div>

              <div class="field">
                <label for="pickupNotes">Extra notes</label>
                <textarea id="pickupNotes"></textarea>
              </div>

              <div class="split-actions" id="userBookingActions">
                <button class="btn btn-primary" type="submit">Book Pickup</button>
                <button class="btn btn-secondary" type="button" id="clearBookingBtn">Clear Form</button>
              </div>
            </form>

            <div id="pickupMessage" class="success-box"></div>
            <div id="pickupError" class="error-box"></div>
          </article>

          <article class="panel card recycler-only">
            <div class="panel-header">
              <div>
                <h2 style="font-family:var(--font-display);">Recycler control</h2>
                <p class="muted small">Recycler only manages active request status.</p>
              </div>
            </div>

            <div id="pickupControlBox" class="pickup-summary">
              <p class="muted">No pickup request available.</p>
            </div>

            <div class="split-actions" id="recyclerActions">
              <button class="btn btn-secondary" id="acceptPickupBtn" type="button">Accept Pickup</button>
              <button class="btn btn-primary" id="completePickupBtn" type="button">Mark Completed</button>
              <button class="btn btn-danger" id="clearActivePickupBtn" type="button">Clear Active Pickup</button>
            </div>
          </article>
        </div>
      </section>

      <section id="view-pricing" class="view">
        <article class="panel card">
          <div class="panel-header">
            <div>
              <h2 style="font-family:var(--font-display);">Price board</h2>
              <p class="muted small">Recycler edits rates, user sees values.</p>
            </div>
          </div>

          <div class="price-grid">
            <div class="field"><label for="pricePlastic">Plastic</label><input id="pricePlastic" type="number"></div>
            <div class="field"><label for="pricePaper">Paper</label><input id="pricePaper" type="number"></div>
            <div class="field"><label for="priceMetal">Metal</label><input id="priceMetal" type="number"></div>
            <div class="field"><label for="priceGlass">Glass</label><input id="priceGlass" type="number"></div>
            <div class="field"><label for="priceEwaste">E-Waste</label><input id="priceEwaste" type="number"></div>
          </div>

          <div class="split-actions">
            <button class="btn btn-primary recycler-only" id="savePricesBtn" type="button">Save Prices</button>
            <button class="btn btn-secondary" id="refreshEstimateBtn" type="button">Refresh Estimate</button>
          </div>

          <div id="priceMessage" class="success-box"></div>
        </article>
      </section>

      <section id="view-mapView" class="view">
        <article class="panel card">
          <div class="panel-header">
            <div>
              <h2 style="font-family:var(--font-display);">User location</h2>
              <p class="muted small">Shows the live/current user position for recycler tracking.</p>
            </div>
          </div>
          <div class="split-actions" style="margin-bottom:1rem;">
            <button class="btn btn-primary user-only" id="shareLocationBtn" type="button">Share My Location</button>
            <button class="btn btn-secondary recycler-only" id="trackUserBtn" type="button">Track User Location</button>
          </div>
          <div id="map"></div>
          <div id="mapNote" class="note-box"></div>
        </article>
      </section>

      <section id="view-scanner" class="view user-only">
        <div class="two-col">
          <article class="panel card">
            <div class="panel-header">
              <div>
                <h2 style="font-family:var(--font-display);">Waste scanner</h2>
                <p class="muted small">User-only waste image scanning.</p>
              </div>
            </div>

            <div class="scanner-drop">
              <p class="muted">Upload an image for preview and waste detection.</p>
              <div class="split-actions" style="justify-content:center;">
                <label class="btn btn-primary" for="wasteImage">Choose Image</label>
                <input id="wasteImage" type="file" accept="image/*" class="hidden">
                <button class="btn btn-secondary" id="runScannerBtn" type="button">Run Scan</button>
              </div>
            </div>

            <div class="preview-box" id="imagePreviewBox">
              <p class="muted">No image selected.</p>
            </div>
          </article>

          <article class="panel card">
            <div class="panel-header">
              <div>
                <h2 style="font-family:var(--font-display);">Scan result</h2>
                <p class="muted small">User-only scan output.</p>
              </div>
            </div>
            <div class="scanner-results" id="scannerResults">
              <div class="scan-item">
                <h4>No result yet</h4>
                <p class="small muted">Select image and run scan.</p>
              </div>
            </div>
          </article>
        </div>
      </section>

      <section id="view-notifications" class="view">
        <article class="panel card">
          <div class="panel-header">
            <div>
              <h2 style="font-family:var(--font-display);">Notifications</h2>
              <p class="muted small">System updates and request alerts.</p>
            </div>
            <button class="btn btn-secondary" id="clearNotificationsBtn" type="button">Clear Notifications</button>
          </div>
          <div class="notif-list" id="notifList"></div>
        </article>
      </section>

      <section id="view-chat" class="view">
        <article class="panel card">
          <div class="panel-header">
            <div>
              <h2 style="font-family:var(--font-display);">Messages</h2>
              <p class="muted small">Simple user and recycler chat.</p>
            </div>
          </div>
          <div class="chat-list" id="chatList"></div>
          <div class="chat-compose">
            <input id="chatInput" type="text" placeholder="Type a message..." />
            <button class="btn btn-primary" id="sendChatBtn" type="button">Send</button>
          </div>
        </article>
      </section>

      <section id="view-history" class="view recycler-only">
        <article class="panel card">
          <div class="panel-header">
            <div>
              <h2 style="font-family:var(--font-display);">Recycler history</h2>
              <p class="muted small">Past handled pickup records.</p>
            </div>
            <button class="btn btn-secondary" id="clearHistoryBtn" type="button">Clear History</button>
          </div>
          <div class="history-list" id="historyList"></div>
        </article>
      </section>

      <div class="mobile-bar">
        <div class="mobile-nav">
          <button type="button" data-view="dashboard">Home</button>
          <button type="button" data-view="pickup">Pickup</button>
          <button type="button" data-view="pricing">Prices</button>
          <button type="button" data-view="chat">Chat</button>
        </div>
      </div>
    </main>
  </section>

  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script>
    const RECYCLER_SECRET = "7204326807";

    const state = {
      user: null,
      role: "user",
      prices: { plastic: 14, paper: 10, metal: 26, glass: 8, ewaste: 40 },
      currentPickup: null,
      history: [],
      notifications: [
        { title: "Welcome", text: "Final role-based dashboard loaded.", time: new Date().toLocaleString() }
      ],
      chat: [],
      selectedImage: "",
      userLocation: null
    };

    const $ = id => document.getElementById(id);

    const viewsMeta = {
      dashboard: ["Dashboard", "Role-based overview of current recycling activity."],
      pickup: ["Pickup Flow", "User booking remains. Recycler only handles active requests."],
      pricing: ["Price Board", "Review and manage live material pricing."],
      mapView: ["User Location", "Track where the current user is present."],
      scanner: ["Waste Scanner", "User-only image waste detection section."],
      notifications: ["Notifications", "View alerts and workflow updates."],
      chat: ["Messages", "Simple communication interface."],
      history: ["Recycler History", "View handled pickup records."]
    };

    let currentTheme = window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light";
    document.documentElement.setAttribute("data-theme", currentTheme);
    $("themeToggle").textContent = currentTheme === "dark" ? "☀️" : "🌙";

    function toggleTheme() {
      currentTheme = currentTheme === "dark" ? "light" : "dark";
      document.documentElement.setAttribute("data-theme", currentTheme);
      $("themeToggle").textContent = currentTheme === "dark" ? "☀️" : "🌙";
      setTimeout(() => map && map.invalidateSize(), 150);
    }

    ["themeToggle", "sidebarThemeBtn", "topThemeBtn"].forEach(id => {
      $(id).addEventListener("click", toggleTheme);
    });

    function showBox(id, message) {
      const el = $(id);
      el.textContent = message;
      el.classList.add("show");
    }

    function hideBox(id) {
      $(id).textContent = "";
      $(id).classList.remove("show");
    }

    function setRole(role) {
      state.role = role;
      $("userRoleBtn").classList.toggle("active", role === "user");
      $("recyclerRoleBtn").classList.toggle("active", role === "recycler");
      $("secretFieldWrap").classList.toggle("hidden", role !== "recycler");
      $("secretCode").required = role === "recycler";
      hideBox("loginError");
      hideBox("loginSuccess");
    }

    $("userRoleBtn").addEventListener("click", () => setRole("user"));
    $("recyclerRoleBtn").addEventListener("click", () => setRole("recycler"));

    $("loginForm").addEventListener("submit", (e) => {
      e.preventDefault();

      const name = $("loginName").value.trim();
      const phone = $("loginPhone").value.trim();
      const secret = $("secretCode").value.trim();

      if (!name || !phone) {
        showBox("loginError", "Please enter name and phone number.");
        return;
      }

      if (state.role === "recycler" && secret !== RECYCLER_SECRET) {
        showBox("loginError", "Incorrect recycler secret code.");
        return;
      }

      state.user = { name, phone };
      showBox("loginSuccess", "Login successful. Loading dashboard...");
      setTimeout(() => {
        $("loginPage").classList.add("hidden");
        $("appPage").classList.remove("hidden");
        applyRoleUI();
        navigateTo("dashboard");
        renderAll();
      }, 500);
    });

    function logout() {
      state.user = null;
      state.role = "user";
      $("loginForm").reset();
      setRole("user");
      $("appPage").classList.add("hidden");
      $("loginPage").classList.remove("hidden");
    }

    $("logoutBtn").addEventListener("click", logout);
    $("topLogoutBtn").addEventListener("click", logout);

    function applyRoleUI() {
      const isRecycler = state.role === "recycler";

      document.querySelectorAll(".recycler-only").forEach(el => el.classList.toggle("hidden", !isRecycler));
      document.querySelectorAll(".user-only").forEach(el => el.classList.toggle("hidden", isRecycler));

      $("roleLabel").textContent = isRecycler ? "Recycler dashboard" : "User dashboard";
      $("rolePill").textContent = isRecycler ? "Recycler Portal" : "User Portal";
      $("welcomeName").textContent = `Welcome, ${state.user?.name || ""}`;
      $("welcomeMeta").textContent = isRecycler
        ? "Handle pickup status, rates, user location, and history."
        : "Book pickups, scan waste, and track your request.";

      ["pricePlastic","pricePaper","priceMetal","priceGlass","priceEwaste"].forEach(id => {
        $(id).readOnly = !isRecycler;
      });
    }

    function navigateTo(view) {
      if (state.role === "recycler" && view === "scanner") view = "dashboard";
      if (state.role !== "recycler" && view === "history") view = "dashboard";

      document.querySelectorAll(".view").forEach(v => v.classList.remove("active"));
      const target = document.getElementById(`view-${view}`);
      if (target) target.classList.add("active");

      document.querySelectorAll(".nav-btn").forEach(btn => {
        btn.classList.toggle("active", btn.dataset.view === view);
      });

      $("pageTitle").textContent = viewsMeta[view][0];
      $("pageSubtitle").textContent = viewsMeta[view][1];

      if (view === "mapView") setTimeout(() => map && map.invalidateSize(), 200);
    }

    document.querySelectorAll(".nav-btn").forEach(btn => {
      btn.addEventListener("click", () => navigateTo(btn.dataset.view));
    });

    document.querySelectorAll(".mobile-nav button").forEach(btn => {
      btn.addEventListener("click", () => navigateTo(btn.dataset.view));
    });

    function getRate(type) {
      return Number(state.prices[type] || 0);
    }

    function getCurrentPickupValue() {
      if (!state.currentPickup) return 0;
      return Math.round(getRate(state.currentPickup.type) * Number(state.currentPickup.weight));
    }

    function addNotification(title, text) {
      state.notifications.unshift({ title, text, time: new Date().toLocaleString() });
    }

    function renderDashboard() {
      const statsWrap = $("dashboardStats");
      const quickWrap = $("dashboardQuickCards");
      const isRecycler = state.role === "recycler";

      const status = state.currentPickup ? state.currentPickup.status : "No Booking";
      const totalPickups = state.history.length;
      const completedPickups = state.history.filter(x => x.status === "Completed").length;
      const estimatedValue = getCurrentPickupValue();
      const userLocationText = state.userLocation ? "Shared" : "Unknown";

      $("overviewSubtitle").textContent = isRecycler
        ? "Track active request, user location, and recycler workflow."
        : "Track your booking, estimated value, and current request.";

      $("quickSummarySubtitle").textContent = isRecycler
        ? "Recycler-only dashboard stats."
        : "User-only dashboard stats.";

      if (isRecycler) {
        statsWrap.innerHTML = `
          <article class="stat-card card">
            <h3>Active request</h3>
            <div class="stat-value">${status}</div>
            <p class="small muted">Current pickup workflow state.</p>
          </article>
          <article class="stat-card card">
            <h3>User location</h3>
            <div class="stat-value">${userLocationText}</div>
            <p class="small muted">Shared by current user.</p>
          </article>
          <article class="stat-card card">
            <h3>Completed pickups</h3>
            <div class="stat-value">${completedPickups}</div>
            <p class="small muted">Successfully finished requests.</p>
          </article>
          <article class="stat-card card">
            <h3>Total records</h3>
            <div class="stat-value">${totalPickups}</div>
            <p class="small muted">Handled and stored requests.</p>
          </article>
        `;

        quickWrap.innerHTML = `
          <div class="kpi-mini">Pending request <strong>${state.currentPickup && state.currentPickup.status !== "Completed" ? "1" : "0"}</strong></div>
          <div class="kpi-mini">User tracked <strong>${state.userLocation ? "Yes" : "No"}</strong></div>
          <div class="kpi-mini">Price board <strong>Live</strong></div>
        `;
      } else {
        statsWrap.innerHTML = `
          <article class="stat-card card">
            <h3>Current status</h3>
            <div class="stat-value">${status}</div>
            <p class="small muted">${state.currentPickup ? "Your latest pickup request is active." : "Create a pickup request to begin."}</p>
          </article>
          <article class="stat-card card">
            <h3>Estimated value</h3>
            <div class="stat-value">₹${estimatedValue}</div>
            <p class="small muted">Based on selected material and weight.</p>
          </article>
          <article class="stat-card card">
            <h3>Location status</h3>
            <div class="stat-value">${userLocationText}</div>
            <p class="small muted">Share your current location for recycler tracking.</p>
          </article>
          <article class="stat-card card">
            <h3>Total pickups</h3>
            <div class="stat-value">${totalPickups}</div>
            <p class="small muted">Requests created in your account.</p>
          </article>
        `;

        quickWrap.innerHTML = `
          <div class="kpi-mini">Pending request <strong>${state.currentPickup && state.currentPickup.status !== "Completed" ? "1" : "0"}</strong></div>
          <div class="kpi-mini">Current value <strong>₹${estimatedValue}</strong></div>
          <div class="kpi-mini">Location shared <strong>${state.userLocation ? "Yes" : "No"}</strong></div>
        `;
      }

      if (state.currentPickup) {
        $("latestPickupCard").innerHTML = `
          <h4>${state.currentPickup.type.toUpperCase()} pickup</h4>
          <p class="small muted">Weight: ${state.currentPickup.weight} kg</p>
          <p class="small muted">Pickup date: ${state.currentPickup.date}</p>
          <p class="small muted">Address: ${state.currentPickup.address}</p>
          <p class="small muted">Estimated value: ₹${estimatedValue}</p>
        `;

        $("statusBadge").className = "status-badge " + (
          state.currentPickup.status === "Pending" ? "status-pending" :
          state.currentPickup.status === "Accepted" ? "status-accepted" :
          "status-completed"
        );
        $("statusBadge").textContent = state.currentPickup.status;
      } else {
        $("latestPickupCard").innerHTML = `<p class="muted">No active pickup yet.</p>`;
        $("statusBadge").className = "status-badge status-pending";
        $("statusBadge").textContent = "No active booking";
      }
    }

    function renderPickupControl() {
      if (!$("pickupControlBox")) return;
      if (!state.currentPickup) {
        $("pickupControlBox").innerHTML = `<p class="muted">No pickup request available.</p>`;
        return;
      }

      $("pickupControlBox").innerHTML = `
        <h4>${state.currentPickup.type.toUpperCase()} | ${state.currentPickup.weight} kg</h4>
        <p class="small muted">Status: ${state.currentPickup.status}</p>
        <p class="small muted">Date: ${state.currentPickup.date}</p>
        <p class="small muted">Address: ${state.currentPickup.address}</p>
        <p class="small muted">Notes: ${state.currentPickup.notes || "None"}</p>
        <p class="small muted">Estimated value: ₹${getCurrentPickupValue()}</p>
      `;
    }

    $("pickupForm").addEventListener("submit", (e) => {
      e.preventDefault();

      const pickup = {
        id: Date.now(),
        type: $("wasteType").value,
        weight: Number($("wasteWeight").value),
        date: $("pickupDate").value,
        address: $("pickupAddress").value.trim(),
        notes: $("pickupNotes").value.trim(),
        status: "Pending",
        createdBy: state.user.name
      };

      if (!pickup.type || !pickup.weight || !pickup.date || !pickup.address) {
        showBox("pickupError", "Please fill all required fields.");
        return;
      }

      state.currentPickup = pickup;
      hideBox("pickupError");
      showBox("pickupMessage", "Pickup booked successfully.");
      addNotification("Pickup booked", `A new ${pickup.type} pickup request has been created.`);
      renderAll();
    });

    $("clearBookingBtn").addEventListener("click", () => {
      $("pickupForm").reset();
      hideBox("pickupMessage");
      hideBox("pickupError");
    });

    $("acceptPickupBtn").addEventListener("click", () => {
      if (!state.currentPickup) return;
      state.currentPickup.status = "Accepted";
      state.currentPickup.handledBy = state.user.name;
      addNotification("Pickup accepted", "Recycler accepted the active pickup.");
      renderAll();
    });

    $("completePickupBtn").addEventListener("click", () => {
      if (!state.currentPickup) return;
      state.currentPickup.status = "Completed";
      state.currentPickup.handledBy = state.user.name;
      state.history.unshift({ ...state.currentPickup, value: getCurrentPickupValue() });
      addNotification("Pickup completed", "The active pickup has been completed.");
      state.currentPickup = null;
      renderAll();
    });

    $("clearActivePickupBtn").addEventListener("click", () => {
      if (!state.currentPickup) return;
      state.history.unshift({ ...state.currentPickup, status: "Cleared" });
      addNotification("Pickup cleared", "Active pickup was cleared.");
      state.currentPickup = null;
      renderAll();
    });

    function renderPrices() {
      $("pricePlastic").value = state.prices.plastic;
      $("pricePaper").value = state.prices.paper;
      $("priceMetal").value = state.prices.metal;
      $("priceGlass").value = state.prices.glass;
      $("priceEwaste").value = state.prices.ewaste;
    }

    $("savePricesBtn").addEventListener("click", () => {
      state.prices = {
        plastic: Number($("pricePlastic").value || 0),
        paper: Number($("pricePaper").value || 0),
        metal: Number($("priceMetal").value || 0),
        glass: Number($("priceGlass").value || 0),
        ewaste: Number($("priceEwaste").value || 0)
      };
      showBox("priceMessage", "Prices saved successfully.");
      addNotification("Prices updated", "Recycler updated the live price board.");
      renderAll();
    });

    $("refreshEstimateBtn").addEventListener("click", () => {
      renderDashboard();
      showBox("priceMessage", "Estimate refreshed.");
    });

    function renderNotifications() {
      $("notifList").innerHTML = state.notifications.length
        ? state.notifications.map(item => `
          <div class="notif-item">
            <h4>${item.title}</h4>
            <p class="small muted">${item.text}</p>
            <p class="tiny muted">${item.time}</p>
          </div>
        `).join("")
        : `<div class="notif-item"><p class="muted">No notifications available.</p></div>`;
    }

    function renderChat() {
      $("chatList").innerHTML = state.chat.length
        ? state.chat.map(msg => `
          <div class="chat-item">
            <h4>${msg.sender}</h4>
            <p class="small muted">${msg.text}</p>
            <p class="tiny muted">${msg.time}</p>
          </div>
        `).join("")
        : `<div class="chat-item"><p class="muted">No messages yet.</p></div>`;
    }

    function sendChat() {
      const text = $("chatInput").value.trim();
      if (!text) return;
      state.chat.unshift({
        sender: state.role === "recycler" ? `Recycler: ${state.user.name}` : `User: ${state.user.name}`,
        text,
        time: new Date().toLocaleString()
      });
      $("chatInput").value = "";
      renderChat();
    }

    $("sendChatBtn").addEventListener("click", sendChat);
    $("chatInput").addEventListener("keydown", (e) => {
      if (e.key === "Enter") sendChat();
    });

    function renderHistory() {
      $("historyList").innerHTML = state.history.length
        ? state.history.map(item => `
          <div class="history-item">
            <h4>${item.type.toUpperCase()} - ${item.status}</h4>
            <p class="small muted">Weight: ${item.weight} kg</p>
            <p class="small muted">Address: ${item.address || "N/A"}</p>
            <p class="small muted">Handled by: ${item.handledBy || item.createdBy || "N/A"}</p>
            <p class="small muted">Value: ₹${item.value || 0}</p>
          </div>
        `).join("")
        : `<div class="history-item"><p class="muted">No recycler history available.</p></div>`;
    }

    $("clearNotificationsBtn").addEventListener("click", () => {
      state.notifications = [];
      renderNotifications();
      showBox("mapNote", "All notifications cleared.");
    });

    $("clearHistoryBtn").addEventListener("click", () => {
      state.history = [];
      renderHistory();
      renderDashboard();
      showBox("mapNote", "Recycler history cleared.");
    });

    $("wasteImage").addEventListener("change", (e) => {
      const file = e.target.files?.[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = () => {
        state.selectedImage = reader.result;
        $("imagePreviewBox").innerHTML = `<img src="${reader.result}" alt="Uploaded waste preview">`;
      };
      reader.readAsDataURL(file);
    });

    $("runScannerBtn").addEventListener("click", () => {
      if (!state.selectedImage) {
        $("scannerResults").innerHTML = `
          <div class="scan-item">
            <h4>No image selected</h4>
            <p class="small muted">Choose image before running scan.</p>
          </div>
        `;
        return;
      }

      const labels = [
        { item: "Plastic Bottle", type: "plastic", confidence: 94, value: 12 },
        { item: "Paper Bundle", type: "paper", confidence: 89, value: 8 },
        { item: "Metal Can", type: "metal", confidence: 92, value: 18 },
        { item: "Glass Piece", type: "glass", confidence: 87, value: 6 },
        { item: "Old Charger", type: "ewaste", confidence: 91, value: 24 }
      ];

      const result = labels[Math.floor(Math.random() * labels.length)];
      $("scannerResults").innerHTML = `
        <div class="scan-item">
          <h4>${result.item}</h4>
          <p class="small muted">Detected category: ${result.type.toUpperCase()}</p>
          <p class="small muted">Confidence: ${result.confidence}%</p>
          <p class="small muted">Estimated value: ₹${result.value}</p>
        </div>
      `;
      addNotification("Waste scan complete", `${result.item} detected with ${result.confidence}% confidence.`);
      renderNotifications();
    });

    let map, mapMarker;

    function initMap() {
      map = L.map("map").setView([13.277, 77.545], 13);
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        maxZoom: 19,
        attribution: "&copy; OpenStreetMap contributors"
      }).addTo(map);

      mapMarker = L.marker([13.277, 77.545]).addTo(map).bindPopup("Waiting for user location").openPopup();
    }

    $("shareLocationBtn").addEventListener("click", () => {
      if (!navigator.geolocation) {
        showBox("mapNote", "Geolocation is not supported.");
        return;
      }

      navigator.geolocation.getCurrentPosition(
        (pos) => {
          const { latitude, longitude } = pos.coords;
          state.userLocation = { latitude, longitude };
          map.setView([latitude, longitude], 15);
          mapMarker.setLatLng([latitude, longitude]).bindPopup("User current location").openPopup();
          showBox("mapNote", `User location shared: ${latitude.toFixed(4)}, ${longitude.toFixed(4)}`);
          addNotification("Location shared", "User shared current location for recycler tracking.");
          renderDashboard();
          renderNotifications();
        },
        () => showBox("mapNote", "Unable to fetch user location.")
      );
    });

    $("trackUserBtn").addEventListener("click", () => {
      if (!state.userLocation) {
        showBox("mapNote", "User has not shared location yet.");
        return;
      }

      map.setView([state.userLocation.latitude, state.userLocation.longitude], 15);
      mapMarker
        .setLatLng([state.userLocation.latitude, state.userLocation.longitude])
        .bindPopup("Tracked user location")
        .openPopup();

      showBox("mapNote", "Showing current user location on map.");
    });

    function renderAll() {
      renderDashboard();
      renderPickupControl();
      renderPrices();
      renderNotifications();
      renderChat();
      renderHistory();
    }

    initMap();
    setRole("user");
  </script>
</body>
</html>
