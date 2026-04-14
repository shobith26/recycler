<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ReCycleHub Role Login Dashboard</title>

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet" />

  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    crossorigin=""
  />
  <script
    src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
    crossorigin=""
  ></script>

  <style>
    :root{
      --bg:#ecf8f2;
      --surface:#ffffff;
      --surface-2:#f5fbf8;
      --text:#16352b;
      --muted:#6b847a;
      --primary:#0f9d74;
      --primary-dark:#0b7c5b;
      --accent:#22c58b;
      --danger:#ef4444;
      --info:#2563eb;
      --warn:#f59e0b;
      --border:1px solid rgba(20,60,45,0.10);
      --shadow:0 12px 32px rgba(15,40,30,0.10);
      --radius:20px;
    }

    *{box-sizing:border-box;margin:0;padding:0}
    body{
      font-family:'Inter',sans-serif;
      background:linear-gradient(180deg,#eefaf4,#e5f5ee);
      color:var(--text);
      min-height:100vh;
    }

    .hidden{display:none !important}

    .login-screen{
      min-height:100vh;
      display:flex;
      justify-content:center;
      align-items:center;
      padding:20px;
    }

    .login-card{
      width:100%;
      max-width:480px;
      background:rgba(255,255,255,0.95);
      border:var(--border);
      border-radius:28px;
      box-shadow:var(--shadow);
      padding:32px;
      backdrop-filter:blur(12px);
    }

    .brand{
      display:flex;
      align-items:center;
      gap:14px;
      margin-bottom:26px;
    }

    .brand-icon{
      width:54px;
      height:54px;
      border-radius:18px;
      display:grid;
      place-items:center;
      background:linear-gradient(135deg,var(--primary),var(--accent));
      color:#fff;
      font-size:26px;
      font-weight:800;
      box-shadow:var(--shadow);
    }

    .brand h1{
      font-size:1.5rem;
      font-weight:800;
    }

    .brand p{
      color:var(--muted);
      font-size:0.92rem;
      margin-top:4px;
    }

    .section-title{
      font-size:1rem;
      font-weight:800;
      margin-bottom:12px;
    }

    .role-grid{
      display:grid;
      grid-template-columns:repeat(3,1fr);
      gap:10px;
      margin-bottom:20px;
    }

    .role-option{
      border:var(--border);
      background:var(--surface-2);
      border-radius:16px;
      padding:14px 10px;
      text-align:center;
      cursor:pointer;
      font-weight:700;
      transition:.25s ease;
    }

    .role-option.active{
      background:linear-gradient(135deg,var(--primary),var(--accent));
      color:#fff;
      transform:translateY(-2px);
      box-shadow:0 12px 24px rgba(15,157,116,.22);
    }

    .field{
      margin-bottom:14px;
    }

    .field label{
      display:block;
      font-size:.92rem;
      font-weight:700;
      margin-bottom:8px;
    }

    .input, .num-input, select, textarea{
      width:100%;
      padding:13px 14px;
      border-radius:14px;
      border:var(--border);
      font:inherit;
      outline:none;
      background:#fff;
      color:var(--text);
    }

    .input:focus, .num-input:focus, select:focus, textarea:focus{
      border-color:rgba(15,157,116,.45);
      box-shadow:0 0 0 4px rgba(15,157,116,.10);
    }

    .login-actions{
      display:flex;
      gap:10px;
      margin-top:18px;
      flex-wrap:wrap;
    }

    .btn{
      border:none;
      border-radius:14px;
      padding:12px 16px;
      font-weight:700;
      cursor:pointer;
      transition:.25s ease;
    }

    .btn-primary{
      background:linear-gradient(135deg,var(--primary),var(--accent));
      color:#fff;
      box-shadow:0 12px 24px rgba(15,157,116,.20);
    }

    .btn-secondary{
      background:#e9f7f1;
      color:var(--primary-dark);
    }

    .btn-danger{
      background:#fee2e2;
      color:#b91c1c;
    }

    .btn:hover{
      transform:translateY(-1px);
    }

    .app{
      display:grid;
      grid-template-columns:280px 1fr;
      min-height:100vh;
    }

    .sidebar{
      background:rgba(255,255,255,.88);
      border-right:var(--border);
      padding:22px 18px;
      position:sticky;
      top:0;
      height:100vh;
      backdrop-filter:blur(12px);
    }

    .sidebar .small{
      color:var(--muted);
      font-size:.9rem;
      margin-top:4px;
    }

    .nav{
      display:flex;
      flex-direction:column;
      gap:10px;
      margin-top:24px;
    }

    .nav button{
      border:none;
      background:transparent;
      text-align:left;
      padding:13px 14px;
      border-radius:14px;
      cursor:pointer;
      font:inherit;
      font-weight:700;
      color:var(--text);
      transition:.25s ease;
    }

    .nav button:hover,
    .nav button.active{
      background:#e8f7f1;
      color:var(--primary-dark);
    }

    .profile-card{
      margin-top:28px;
      padding:16px;
      border-radius:18px;
      background:linear-gradient(135deg,var(--primary),var(--accent));
      color:#fff;
      box-shadow:var(--shadow);
    }

    .main{
      padding:24px;
    }

    .topbar{
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:14px;
      flex-wrap:wrap;
      background:rgba(255,255,255,.8);
      border:var(--border);
      border-radius:22px;
      padding:18px 20px;
      margin-bottom:20px;
      backdrop-filter:blur(8px);
    }

    .topbar h2{
      font-size:1.35rem;
      font-weight:800;
    }

    .topbar p{
      color:var(--muted);
      margin-top:4px;
    }

    .pill{
      padding:10px 14px;
      border-radius:999px;
      background:#fff;
      border:var(--border);
      font-size:.9rem;
      font-weight:700;
    }

    .top-actions{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      align-items:center;
    }

    .grid{
      display:grid;
      grid-template-columns:repeat(12,1fr);
      gap:18px;
    }

    .card{
      grid-column:span 12;
      background:rgba(255,255,255,.94);
      border:var(--border);
      border-radius:20px;
      box-shadow:var(--shadow);
      padding:20px;
    }

    .card h3{
      margin-bottom:14px;
      font-size:1.05rem;
      font-weight:800;
    }

    .subtle{
      color:var(--muted);
      font-size:.94rem;
      line-height:1.55;
      margin-bottom:14px;
    }

    .col-3{grid-column:span 3}
    .col-4{grid-column:span 4}
    .col-5{grid-column:span 5}
    .col-6{grid-column:span 6}
    .col-7{grid-column:span 7}
    .col-8{grid-column:span 8}
    .col-12{grid-column:span 12}

    .metrics{
      display:grid;
      grid-template-columns:repeat(4,1fr);
      gap:14px;
    }

    .metric{
      background:linear-gradient(180deg,#ffffff,#f3fbf7);
      border:var(--border);
      border-radius:16px;
      padding:15px;
    }

    .metric .label{
      color:var(--muted);
      font-size:.82rem;
      margin-bottom:8px;
      display:block;
    }

    .metric .num-input{
      font-size:1.4rem;
      font-weight:800;
      border:none;
      padding:0;
      background:transparent;
      color:var(--text);
    }

    .metric .num-input::-webkit-outer-spin-button,
    .metric .num-input::-webkit-inner-spin-button{
      -webkit-appearance:none;
      margin:0;
    }

    .form-grid{
      display:grid;
      grid-template-columns:repeat(2,1fr);
      gap:14px;
    }

    .label{
      display:block;
      font-size:.92rem;
      font-weight:700;
      margin-bottom:8px;
    }

    .btn-row{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      margin-top:14px;
    }

    .badge{
      display:inline-block;
      padding:7px 11px;
      border-radius:999px;
      font-size:.84rem;
      font-weight:700;
      margin-right:8px;
      margin-bottom:8px;
    }

    .success{background:#dcfce7;color:#166534}
    .info{background:#dbeafe;color:#1d4ed8}
    .warn{background:#fef3c7;color:#92400e}
    .danger{background:#fee2e2;color:#b91c1c}

    .list{
      display:flex;
      flex-direction:column;
      gap:12px;
    }

    .item{
      border:var(--border);
      border-radius:14px;
      padding:14px;
      background:#fbfefd;
    }

    .item strong{
      display:block;
      margin-bottom:5px;
    }

    .timeline{
      display:flex;
      flex-direction:column;
      gap:12px;
    }

    .timeline-item{
      display:flex;
      gap:12px;
      align-items:flex-start;
      border:var(--border);
      border-radius:14px;
      background:#f8fcfa;
      padding:12px;
    }

    .dot{
      width:12px;
      height:12px;
      border-radius:50%;
      background:var(--accent);
      margin-top:6px;
      flex-shrink:0;
    }

    .chat{
      display:flex;
      flex-direction:column;
      min-height:360px;
      border:var(--border);
      border-radius:16px;
      overflow:hidden;
      background:#fff;
    }

    .chat-messages{
      flex:1;
      background:#f7fcf9;
      padding:14px;
      overflow-y:auto;
      max-height:320px;
    }

    .msg{
      max-width:78%;
      padding:12px 14px;
      border-radius:16px;
      margin-bottom:10px;
      line-height:1.45;
      font-size:.94rem;
    }

    .msg.user{
      margin-left:auto;
      background:linear-gradient(135deg,var(--primary),var(--accent));
      color:#fff;
      border-bottom-right-radius:4px;
    }

    .msg.recycler{
      background:#e8f0ff;
      color:#1e3a8a;
      border-bottom-left-radius:4px;
    }

    .chat-input{
      display:flex;
      gap:10px;
      padding:12px;
      border-top:var(--border);
      background:#fff;
    }

    .table-wrap{
      overflow-x:auto;
      border:var(--border);
      border-radius:16px;
      background:#fff;
    }

    table{
      width:100%;
      min-width:700px;
      border-collapse:collapse;
    }

    th,td{
      padding:14px;
      text-align:left;
      border-bottom:1px solid rgba(20,60,45,.08);
      font-size:.94rem;
    }

    th{
      background:#f2faf6;
      color:var(--primary-dark);
      font-weight:800;
    }

    #map{
      width:100%;
      height:320px;
      border-radius:16px;
      border:var(--border);
      overflow:hidden;
    }

    @media (max-width:1100px){
      .app{grid-template-columns:1fr}
      .sidebar{
        height:auto;
        position:relative;
        border-right:none;
        border-bottom:var(--border);
      }
      .metrics{grid-template-columns:repeat(2,1fr)}
      .col-3,.col-4,.col-5,.col-6,.col-7,.col-8{grid-column:span 12}
    }

    @media (max-width:700px){
      .role-grid{grid-template-columns:1fr}
      .form-grid{grid-template-columns:1fr}
      .metrics{grid-template-columns:1fr}
      .chat-input{flex-direction:column}
      .main{padding:14px}
    }
  </style>
</head>
<body>

  <section class="login-screen" id="loginScreen">
    <div class="login-card">
      <div class="brand">
        <div class="brand-icon">♻</div>
        <div>
          <h1>ReCycleHub</h1>
          <p>Role-based smart waste management dashboard</p>
        </div>
      </div>

      <div class="section-title">Login as</div>
      <div class="role-grid">
        <div class="role-option active" data-login-role="user">👤 User</div>
        <div class="role-option" data-login-role="recycler">🚚 Recycler</div>
        <div class="role-option" data-login-role="admin">🛠 Admin</div>
      </div>

      <div class="field">
        <label for="loginName">Name</label>
        <input class="input" id="loginName" type="text" placeholder="Enter your name" autocomplete="off" />
      </div>

      <div class="field">
        <label for="loginEmail">Email</label>
        <input class="input" id="loginEmail" type="email" placeholder="Enter email" autocomplete="off" />
      </div>

      <div class="field">
        <label for="loginPassword">Password</label>
        <input class="input" id="loginPassword" type="password" placeholder="Enter password" autocomplete="new-password" />
      </div>

      <div class="login-actions">
        <button class="btn btn-primary" id="loginBtn">Login</button>
        <button class="btn btn-secondary" id="fillDemoBtn">Use Demo Credentials</button>
      </div>
    </div>
  </section>

  <section class="app hidden" id="appScreen">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-icon">♻</div>
        <div>
          <h1>ReCycleHub</h1>
          <p class="small">Eco dashboard prototype</p>
        </div>
      </div>

      <div class="pill" id="sidebarRolePill">Role: User</div>

      <div class="nav">
        <button class="nav-btn active" data-view="dashboard">Dashboard</button>
        <button class="nav-btn" data-view="booking">Book Pickup</button>
        <button class="nav-btn" data-view="scan">Waste Scan</button>
        <button class="nav-btn" data-view="chat">Chat</button>
        <button class="nav-btn" id="historyNavBtn" data-view="history">History</button>
      </div>

      <div class="profile-card">
        <strong id="profileName">Shobith Gowda D</strong>
        <p id="profileEmail" style="margin-top:8px;font-size:.92rem;opacity:.95;">shobith@gmail.com</p>
        <p id="profileRole" style="margin-top:6px;font-size:.92rem;opacity:.95;">User access</p>
        <div class="btn-row">
          <button class="btn btn-secondary" id="logoutBtn">Logout</button>
        </div>
      </div>
    </aside>

    <main class="main">
      <div class="topbar">
        <div>
          <h2 id="pageTitle">User Dashboard</h2>
          <p id="pageSubtitle">Track pickups, rewards, and waste scanning.</p>
        </div>
        <div class="top-actions">
          <div class="pill" id="topRolePill">User</div>
          <div class="pill" id="notifCountPill">Notifications: 3</div>
          <button class="btn btn-secondary" id="gpsBtn">Use GPS</button>
        </div>
      </div>

      <section class="view" id="view-dashboard">
        <div class="grid">
          <div class="card col-8">
            <h3>Editable metrics</h3>
            <p class="subtle">All dashboard numbers below can be changed manually. Edit any number based on user, recycler, or admin login.</p>
            <div class="metrics">
              <div class="metric">
                <span class="label" id="metric1Label">Total pickups</span>
                <input class="num-input" id="metric1" type="number" value="12" />
              </div>
              <div class="metric">
                <span class="label" id="metric2Label">Rewards earned</span>
                <input class="num-input" id="metric2" type="number" value="460" />
              </div>
              <div class="metric">
                <span class="label" id="metric3Label">Waste scans</span>
                <input class="num-input" id="metric3" type="number" value="9" />
              </div>
              <div class="metric">
                <span class="label" id="metric4Label">Completion rate %</span>
                <input class="num-input" id="metric4" type="number" value="92" />
              </div>
            </div>
            <div class="btn-row">
              <button class="btn btn-primary" id="saveMetricsBtn">Save numbers</button>
              <button class="btn btn-secondary" id="resetMetricsBtn">Reset by role</button>
            </div>
          </div>

          <div class="card col-4">
            <h3>Notifications</h3>
            <div class="list" id="notificationList"></div>
          </div>

          <div class="card col-7">
            <h3>Location map</h3>
            <p class="subtle">Login with any role, capture location, and use it for pickup or route visibility.</p>
            <div id="map"></div>
            <div class="btn-row">
              <button class="btn btn-primary" id="locateBtn">Locate me</button>
              <button class="btn btn-secondary" id="showRecyclerBtn">Show recycler marker</button>
            </div>
            <p class="subtle" id="locationText" style="margin-top:12px;">Location not captured yet.</p>
          </div>

          <div class="card col-5">
            <h3>Status timeline</h3>
            <div class="timeline" id="timeline"></div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-booking">
        <div class="grid">
          <div class="card col-7">
            <h3>Book pickup</h3>
            <div class="form-grid">
              <div>
                <label class="label">Full name</label>
                <input class="input" id="fullName" />
              </div>
              <div>
                <label class="label">Phone</label>
                <input class="input" id="phone" value="9876543210" />
              </div>
              <div>
                <label class="label">Address</label>
                <input class="input" id="address" value="Bāshettihalli, Karnataka" />
              </div>
              <div>
                <label class="label">Waste type</label>
                <select id="wasteType">
                  <option>Plastic</option>
                  <option>Paper</option>
                  <option>Metal</option>
                  <option>Glass</option>
                  <option>E-waste</option>
                  <option>Battery</option>
                  <option>Mixed</option>
                </select>
              </div>
              <div>
                <label class="label">Weight (kg)</label>
                <input class="num-input" id="weight" type="number" value="5" />
              </div>
              <div>
                <label class="label">Pickup slot</label>
                <select id="slot">
                  <option>10:00 AM - 12:00 PM</option>
                  <option selected>02:00 PM - 04:00 PM</option>
                  <option>05:00 PM - 07:00 PM</option>
                </select>
              </div>
              <div>
                <label class="label">Pickup date</label>
                <input class="input" id="pickupDate" type="date" />
              </div>
              <div>
                <label class="label">Notes</label>
                <input class="input" id="notes" placeholder="Landmark or pickup note" />
              </div>
            </div>
            <div class="btn-row">
              <button class="btn btn-primary" id="bookBtn">Confirm pickup</button>
              <button class="btn btn-secondary" id="fillGpsBtn">Use GPS address</button>
            </div>
          </div>

          <div class="card col-5">
            <h3>Booking summary</h3>
            <div id="bookingSummary">
              <span class="badge info">No booking yet</span>
              <p class="subtle">Booking details will appear here after confirmation.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-scan">
        <div class="grid">
          <div class="card col-6">
            <h3>Waste scan</h3>
            <p class="subtle">Upload an image to simulate waste detection.</p>
            <input class="input" id="scanFile" type="file" accept="image/*" />
            <div class="btn-row">
              <button class="btn btn-primary" id="scanBtn">Scan waste</button>
            </div>
            <img id="previewImage" class="hidden" alt="Preview" style="margin-top:16px;width:100%;max-height:280px;object-fit:cover;border-radius:16px;border:var(--border);" />
          </div>

          <div class="card col-6">
            <h3>Scan result</h3>
            <div id="scanResult">
              <span class="badge warn">Waiting for image</span>
              <p class="subtle">Choose an image and click scan.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-chat">
        <div class="grid">
          <div class="card col-7">
            <h3>Chat</h3>
            <div class="chat">
              <div class="chat-messages" id="chatMessages"></div>
              <div class="chat-input">
                <input class="input" id="chatInput" placeholder="Type a message..." />
                <button class="btn btn-primary" id="sendChatBtn">Send</button>
              </div>
            </div>
          </div>

          <div class="card col-5">
            <h3>Role notes</h3>
            <div id="roleNoteBox">
              <span class="badge success">Logged in</span>
              <p class="subtle">Role-specific notes will appear here.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-history">
        <div class="grid">
          <div class="card col-12">
            <h3>History</h3>
            <div class="btn-row">
              <button class="btn btn-danger" id="clearHistoryBtn">Clear History</button>
            </div>
            <div class="table-wrap">
              <table>
                <thead>
                  <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Waste</th>
                    <th>Weight</th>
                    <th>Status</th>
                    <th>Reward</th>
                  </tr>
                </thead>
                <tbody id="historyBody"></tbody>
              </table>
            </div>
          </div>
        </div>
      </section>
    </main>
  </section>

  <script>
    const ADMIN_CREDENTIALS = {
      name: "Shobith",
      email: "shobith2345@gmail.com",
      password: "7204326807"
    };

    const appState = {
      selectedRole: "user",
      currentRole: "user",
      currentUser: {
        name: "Shobith Gowda D",
        email: "shobith@gmail.com"
      },
      location: null,
      addressText: "",
      notifications: [
        { title: "Pickup booked", body: "Your pickup was added successfully." },
        { title: "Recycler assigned", body: "A recycler is assigned to the request." },
        { title: "Reward credited", body: "Wallet updated after completion." }
      ],
      chats: [
        { sender: "recycler", text: "I will reach in 10 minutes.", time: "2:10 PM" },
        { sender: "user", text: "Okay, I am near the gate.", time: "2:11 PM" }
      ],
      history: [
        { id: "PK001", name: "Shobith", waste: "Plastic", weight: "5 kg", status: "Completed", reward: "₹120" },
        { id: "PK002", name: "Shobith", waste: "E-waste", weight: "2 kg", status: "Completed", reward: "₹210" },
        { id: "PK003", name: "Shobith", waste: "Paper", weight: "3 kg", status: "Scheduled", reward: "₹60" }
      ],
      metricsByRole: {
        user: { l1: "Total pickups", v1: 12, l2: "Rewards earned", v2: 460, l3: "Waste scans", v3: 9, l4: "Completion rate %", v4: 92 },
        recycler: { l1: "Assigned pickups", v1: 18, l2: "Daily earnings", v2: 1250, l3: "Collected kg", v3: 25, l4: "Completion rate %", v4: 96 },
        admin: { l1: "Total users", v1: 247, l2: "System revenue", v2: 54300, l3: "Open pickups", v3: 128, l4: "Resolution rate %", v4: 91 }
      },
      statusFlow: [
        "Pending request",
        "Recycler assigned",
        "On the way",
        "Collected",
        "Reward credited"
      ]
    };

    const rewardMap = {
      plastic: "₹40 - ₹120",
      paper: "₹20 - ₹80",
      metal: "₹60 - ₹180",
      glass: "₹25 - ₹90",
      "e-waste": "₹120 - ₹500",
      battery: "₹70 - ₹250",
      mixed: "₹20 - ₹100"
    };

    let map, userMarker, recyclerMarker;

    function initMap() {
      map = L.map('map').setView([13.045, 77.623], 13);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
      }).addTo(map);
    }

    function updateLoginRoleSelection() {
      document.querySelectorAll(".role-option").forEach(btn => {
        btn.classList.toggle("active", btn.dataset.loginRole === appState.selectedRole);
      });
    }

    function setSelectedRole(role) {
      appState.selectedRole = role;
      updateLoginRoleSelection();
    }

    function updateHistoryVisibility() {
      const historyNavBtn = document.getElementById("historyNavBtn");
      const historyView = document.getElementById("view-history");
      const isUser = appState.currentRole === "user";

      historyNavBtn.classList.toggle("hidden", isUser);
      historyView.classList.toggle("hidden", isUser);
    }

    function applyRoleUI() {
      const role = appState.currentRole;
      const roleTitle = role.charAt(0).toUpperCase() + role.slice(1);

      document.getElementById("sidebarRolePill").textContent = `Role: ${roleTitle}`;
      document.getElementById("topRolePill").textContent = roleTitle;
      document.getElementById("profileRole").textContent = `${roleTitle} access`;

      const pageTitle = document.getElementById("pageTitle");
      const pageSubtitle = document.getElementById("pageSubtitle");
      const roleNoteBox = document.getElementById("roleNoteBox");

      if (role === "user") {
        pageTitle.textContent = "User Dashboard";
        pageSubtitle.textContent = "Track pickups, rewards, and waste scanning.";
        roleNoteBox.innerHTML = `
          <span class="badge success">User mode</span>
          <p class="subtle">Book pickups, scan waste, view rewards, and chat with your recycler.</p>
        `;
      } else if (role === "recycler") {
        pageTitle.textContent = "Recycler Dashboard";
        pageSubtitle.textContent = "Manage assigned pickups, routes, and collection performance.";
        roleNoteBox.innerHTML = `
          <span class="badge info">Recycler mode</span>
          <p class="subtle">Track routes, update pickup status, chat with users, and edit collection numbers.</p>
        `;
      } else {
        pageTitle.textContent = "Admin Dashboard";
        pageSubtitle.textContent = "Monitor operations, users, recyclers, and overall system metrics.";
        roleNoteBox.innerHTML = `
          <span class="badge warn">Admin mode</span>
          <p class="subtle">Monitor system-wide numbers, pickups, roles, and dashboard analytics.</p>
        `;
      }

      loadMetricsForRole(role);
      updateHistoryVisibility();
    }

    function loadMetricsForRole(role) {
      const metrics = appState.metricsByRole[role];
      document.getElementById("metric1Label").textContent = metrics.l1;
      document.getElementById("metric2Label").textContent = metrics.l2;
      document.getElementById("metric3Label").textContent = metrics.l3;
      document.getElementById("metric4Label").textContent = metrics.l4;

      document.getElementById("metric1").value = metrics.v1;
      document.getElementById("metric2").value = metrics.v2;
      document.getElementById("metric3").value = metrics.v3;
      document.getElementById("metric4").value = metrics.v4;
    }

    function saveMetricsForRole() {
      const role = appState.currentRole;
      appState.metricsByRole[role].v1 = Number(document.getElementById("metric1").value || 0);
      appState.metricsByRole[role].v2 = Number(document.getElementById("metric2").value || 0);
      appState.metricsByRole[role].v3 = Number(document.getElementById("metric3").value || 0);
      appState.metricsByRole[role].v4 = Number(document.getElementById("metric4").value || 0);

      appState.notifications.unshift({
        title: "Metrics updated",
        body: `${role.charAt(0).toUpperCase() + role.slice(1)} dashboard numbers were updated.`
      });
      renderNotifications();
      alert("Dashboard numbers updated successfully.");
    }

    function renderNotifications() {
      const list = document.getElementById("notificationList");
      document.getElementById("notifCountPill").textContent = `Notifications: ${appState.notifications.length}`;
      list.innerHTML = appState.notifications.map(n => `
        <div class="item">
          <strong>${n.title}</strong>
          <div style="color:var(--muted);font-size:.9rem;">${n.body}</div>
        </div>
      `).join("");
    }

    function renderTimeline() {
      const timeline = document.getElementById("timeline");
      timeline.innerHTML = appState.statusFlow.map(step => `
        <div class="timeline-item">
          <div class="dot"></div>
          <div>
            <strong>${step}</strong>
            <div style="color:var(--muted);font-size:.9rem;">Workflow event visible on dashboard.</div>
          </div>
        </div>
      `).join("");
    }

    function renderChats() {
      const box = document.getElementById("chatMessages");
      box.innerHTML = appState.chats.map(msg => `
        <div class="msg ${msg.sender}">
          ${msg.text}
          <div style="opacity:.75;margin-top:6px;font-size:.8rem;">${msg.time}</div>
        </div>
      `).join("");
      box.scrollTop = box.scrollHeight;
    }

    function renderHistory() {
      const body = document.getElementById("historyBody");

      if (!appState.history.length) {
        body.innerHTML = `
          <tr>
            <td colspan="6" style="text-align:center;color:var(--muted);padding:20px;">
              No history available
            </td>
          </tr>
        `;
        return;
      }

      body.innerHTML = appState.history.map(item => `
        <tr>
          <td>${item.id}</td>
          <td>${item.name}</td>
          <td>${item.waste}</td>
          <td>${item.weight}</td>
          <td>${item.status}</td>
          <td>${item.reward}</td>
        </tr>
      `).join("");
    }

    function clearHistory() {
      const confirmClear = confirm("Are you sure you want to clear all history?");
      if (!confirmClear) return;

      appState.history = [];
      renderHistory();

      appState.notifications.unshift({
        title: "History cleared",
        body: "All history records were removed."
      });
      renderNotifications();
    }

    function switchView(view) {
      if (view === "history" && appState.currentRole === "user") {
        view = "dashboard";
      }

      document.querySelectorAll(".view").forEach(v => v.classList.add("hidden"));
      document.getElementById(`view-${view}`).classList.remove("hidden");

      document.querySelectorAll(".nav-btn").forEach(btn => {
        btn.classList.toggle("active", btn.dataset.view === view);
      });
    }

    function login() {
      const name = document.getElementById("loginName").value.trim();
      const email = document.getElementById("loginEmail").value.trim();
      const password = document.getElementById("loginPassword").value.trim();

      if (!name || !email || !password) {
        alert("Please fill all login fields.");
        return;
      }

      if (appState.selectedRole === "admin") {
        const isValidAdmin =
          name === ADMIN_CREDENTIALS.name &&
          email === ADMIN_CREDENTIALS.email &&
          password === ADMIN_CREDENTIALS.password;

        if (!isValidAdmin) {
          alert("Invalid admin credentials. Access denied.");
          return;
        }
      }

      appState.currentUser = { name, email };
      appState.currentRole = appState.selectedRole;

      document.getElementById("profileName").textContent = name;
      document.getElementById("profileEmail").textContent = email;
      document.getElementById("fullName").value = name;

      document.getElementById("loginScreen").classList.add("hidden");
      document.getElementById("appScreen").classList.remove("hidden");

      applyRoleUI();
      switchView("dashboard");

      setTimeout(() => {
        if (!map) initMap();
        if (map) map.invalidateSize();
      }, 100);
    }

    function logout() {
      document.getElementById("appScreen").classList.add("hidden");
      document.getElementById("loginScreen").classList.remove("hidden");

      document.getElementById("loginName").value = "";
      document.getElementById("loginEmail").value = "";
      document.getElementById("loginPassword").value = "";

      appState.currentRole = "user";
      appState.selectedRole = "user";
      updateLoginRoleSelection();
    }

    function fillDemoCredentials() {
      const role = appState.selectedRole;

      if (role === "user") {
        document.getElementById("loginName").value = "Shobith Gowda D";
        document.getElementById("loginEmail").value = "user@recyclehub.com";
        document.getElementById("loginPassword").value = "123456";
      } else if (role === "recycler") {
        document.getElementById("loginName").value = "Ravi Recycler";
        document.getElementById("loginEmail").value = "recycler@recyclehub.com";
        document.getElementById("loginPassword").value = "123456";
      } else {
        document.getElementById("loginName").value = "";
        document.getElementById("loginEmail").value = "";
        document.getElementById("loginPassword").value = "";
        alert("Admin credentials must be entered manually.");
      }
    }

    function useGPS() {
      if (!navigator.geolocation) {
        alert("Geolocation not supported.");
        return;
      }

      navigator.geolocation.getCurrentPosition((pos) => {
        const { latitude, longitude, accuracy } = pos.coords;
        appState.location = { latitude, longitude, accuracy };
        appState.addressText = `Lat ${latitude.toFixed(5)}, Lng ${longitude.toFixed(5)} (accuracy ${Math.round(accuracy)}m)`;

        if (userMarker) map.removeLayer(userMarker);
        userMarker = L.marker([latitude, longitude]).addTo(map).bindPopup("Your location").openPopup();
        map.setView([latitude, longitude], 16);

        document.getElementById("locationText").textContent = `Captured location: ${appState.addressText}`;
        document.getElementById("address").value = appState.addressText;

        appState.notifications.unshift({
          title: "GPS updated",
          body: "Current location captured successfully."
        });
        renderNotifications();
      }, () => {
        alert("Unable to fetch location.");
      });
    }

    function showRecyclerMarker() {
      if (!map) return;
      const recyclerCoords = [13.052, 77.628];
      if (recyclerMarker) map.removeLayer(recyclerMarker);
      recyclerMarker = L.marker(recyclerCoords).addTo(map).bindPopup("Recycler location");
    }

    function bookPickup() {
      const name = document.getElementById("fullName").value.trim();
      const wasteType = document.getElementById("wasteType").value;
      const weight = document.getElementById("weight").value;
      const slot = document.getElementById("slot").value;
      const date = document.getElementById("pickupDate").value;
      const address = document.getElementById("address").value.trim();

      if (!name || !weight || !date || !address) {
        alert("Please fill required booking details.");
        return;
      }

      document.getElementById("bookingSummary").innerHTML = `
        <span class="badge success">Pickup confirmed</span>
        <p class="subtle"><strong>${name}</strong> booked <strong>${wasteType}</strong> pickup of <strong>${weight} kg</strong> on <strong>${date}</strong> during <strong>${slot}</strong>.</p>
        <p class="subtle">Address: ${address}</p>
      `;

      appState.notifications.unshift({
        title: "Pickup confirmed",
        body: `${wasteType} pickup booked for ${date}.`
      });

      appState.history.unshift({
        id: "PK" + String(Math.floor(Math.random() * 900 + 100)),
        name,
        waste: wasteType,
        weight: `${weight} kg`,
        status: "Scheduled",
        reward: rewardMap[wasteType.toLowerCase()] || "₹0"
      });

      renderNotifications();
      renderHistory();
    }

    function previewScanImage(file) {
      const img = document.getElementById("previewImage");
      if (!file) {
        img.classList.add("hidden");
        img.src = "";
        return;
      }
      img.src = URL.createObjectURL(file);
      img.classList.remove("hidden");
    }

    function scanWaste() {
      const fileInput = document.getElementById("scanFile");
      const resultBox = document.getElementById("scanResult");

      if (!fileInput.files.length) {
        alert("Please choose an image first.");
        return;
      }

      const wasteTypes = ["Plastic", "Paper", "Metal", "Glass", "E-waste", "Battery"];
      const selected = wasteTypes[Math.floor(Math.random() * wasteTypes.length)];
      const confidence = (Math.random() * 20 + 80).toFixed(1);

      resultBox.innerHTML = `
        <span class="badge success">Detected: ${selected}</span>
        <p class="subtle">Confidence: <strong>${confidence}%</strong></p>
        <p class="subtle">Estimated reward: <strong>${rewardMap[selected.toLowerCase()]}</strong></p>
      `;

      appState.notifications.unshift({
        title: "Waste scanned",
        body: `${selected} detected with ${confidence}% confidence.`
      });
      renderNotifications();
    }

    function sendChat() {
      const input = document.getElementById("chatInput");
      const text = input.value.trim();
      if (!text) return;

      appState.chats.push({
        sender: "user",
        text,
        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      });

      renderChats();
      input.value = "";

      setTimeout(() => {
        appState.chats.push({
          sender: "recycler",
          text: "Received. I will update you shortly.",
          time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
        });
        renderChats();
      }, 800);
    }

    document.querySelectorAll(".role-option").forEach(btn => {
      btn.addEventListener("click", () => setSelectedRole(btn.dataset.loginRole));
    });

    document.querySelectorAll(".nav-btn").forEach(btn => {
      btn.addEventListener("click", () => switchView(btn.dataset.view));
    });

    document.getElementById("loginBtn").addEventListener("click", login);
    document.getElementById("fillDemoBtn").addEventListener("click", fillDemoCredentials);
    document.getElementById("logoutBtn").addEventListener("click", logout);
    document.getElementById("gpsBtn").addEventListener("click", useGPS);
    document.getElementById("locateBtn").addEventListener("click", useGPS);
    document.getElementById("showRecyclerBtn").addEventListener("click", showRecyclerMarker);
    document.getElementById("bookBtn").addEventListener("click", bookPickup);
    document.getElementById("fillGpsBtn").addEventListener("click", () => {
      document.getElementById("address").value = appState.addressText || "GPS not captured yet";
    });
    document.getElementById("scanBtn").addEventListener("click", scanWaste);
    document.getElementById("sendChatBtn").addEventListener("click", sendChat);
    document.getElementById("saveMetricsBtn").addEventListener("click", saveMetricsForRole);
    document.getElementById("resetMetricsBtn").addEventListener("click", () => loadMetricsForRole(appState.currentRole));
    document.getElementById("clearHistoryBtn").addEventListener("click", clearHistory);

    document.getElementById("scanFile").addEventListener("change", (e) => {
      previewScanImage(e.target.files[0]);
    });

    document.getElementById("chatInput").addEventListener("keypress", (e) => {
      if (e.key === "Enter") sendChat();
    });

    renderNotifications();
    renderTimeline();
    renderChats();
    renderHistory();
    updateLoginRoleSelection();
  </script>
</body>
</html>
