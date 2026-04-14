<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ReCycleHub</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@500;600;700;800&family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">

  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/mobilenet@latest"></script>

  <style>
    :root{
      --bg:#f4fbf7;
      --bg-soft:#edf8f2;
      --card:#ffffffcc;
      --card-solid:#ffffff;
      --text:#15352c;
      --muted:#6f8a80;
      --line:rgba(21,53,44,.10);
      --primary:#0f9d74;
      --primary-2:#22c58b;
      --secondary:#6be0b3;
      --danger:#e74c3c;
      --warning:#f59e0b;
      --blue:#3b82f6;
      --shadow:0 10px 30px rgba(12, 61, 45, 0.08);
      --shadow-lg:0 18px 50px rgba(12, 61, 45, 0.14);
      --radius:22px;
      --radius-sm:14px;
    }

    *{box-sizing:border-box;margin:0;padding:0}
    body{
      font-family:'Poppins',sans-serif;
      background:
        radial-gradient(circle at top left, rgba(34,197,139,.12), transparent 30%),
        radial-gradient(circle at top right, rgba(59,130,246,.08), transparent 25%),
        linear-gradient(180deg,#f8fffb 0%, #eefaf4 100%);
      color:var(--text);
      min-height:100vh;
    }

    h1,h2,h3,h4,h5{
      font-family:'Outfit',sans-serif;
      letter-spacing:.2px;
    }

    button,input,select,textarea{font:inherit}
    .hidden{display:none !important}

    .login-shell{
      min-height:100vh;
      display:grid;
      place-items:center;
      padding:24px;
    }

    .login-card{
      width:min(100%, 980px);
      display:grid;
      grid-template-columns:1.1fr .9fr;
      background:var(--card);
      backdrop-filter:blur(16px);
      border:1px solid var(--line);
      border-radius:32px;
      overflow:hidden;
      box-shadow:var(--shadow-lg);
    }

    .login-hero{
      padding:42px;
      background:
        linear-gradient(135deg, rgba(15,157,116,.96), rgba(34,197,139,.88)),
        linear-gradient(180deg,#0f9d74,#22c58b);
      color:#fff;
      position:relative;
    }

    .login-hero::after{
      content:"";
      position:absolute;
      width:260px;
      height:260px;
      right:-60px;
      bottom:-60px;
      border-radius:50%;
      background:rgba(255,255,255,.10);
      filter:blur(6px);
    }

    .brand{
      display:flex;
      align-items:center;
      gap:14px;
      margin-bottom:28px;
    }

    .brand-mark{
      width:58px;
      height:58px;
      border-radius:18px;
      display:grid;
      place-items:center;
      background:rgba(255,255,255,.18);
      border:1px solid rgba(255,255,255,.2);
      font-size:28px;
      font-weight:800;
      box-shadow:0 12px 24px rgba(0,0,0,.10);
    }

    .brand-text h1{font-size:2rem;font-weight:800}
    .brand-text p{opacity:.9;margin-top:4px}

    .hero-title{
      font-size:2.8rem;
      line-height:1.05;
      margin-bottom:16px;
      max-width:10ch;
    }

    .hero-copy{
      font-size:1rem;
      line-height:1.7;
      max-width:54ch;
      opacity:.95;
      margin-bottom:26px;
    }

    .hero-points{
      display:grid;
      gap:12px;
    }

    .hero-point{
      background:rgba(255,255,255,.10);
      border:1px solid rgba(255,255,255,.12);
      padding:14px 16px;
      border-radius:16px;
    }

    .login-panel{
      padding:42px 34px;
      background:rgba(255,255,255,.78);
    }

    .panel-title{
      font-size:1.8rem;
      margin-bottom:8px;
    }

    .panel-sub{
      color:var(--muted);
      margin-bottom:24px;
      line-height:1.6;
    }

    .role-switch{
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:10px;
      margin-bottom:18px;
    }

    .role-btn{
      border:none;
      border-radius:16px;
      padding:14px;
      cursor:pointer;
      font-weight:700;
      transition:.25s ease;
      background:#eef7f2;
      color:var(--text);
    }

    .role-btn.active{
      background:linear-gradient(135deg,var(--primary),var(--primary-2));
      color:#fff;
      box-shadow:0 10px 22px rgba(15,157,116,.22);
    }

    .field{margin-bottom:14px}
    .field label{
      display:block;
      font-size:.92rem;
      font-weight:600;
      margin-bottom:8px;
    }

    .input{
      width:100%;
      border:1px solid var(--line);
      background:#fff;
      border-radius:16px;
      padding:14px 15px;
      outline:none;
      transition:.22s ease;
    }

    .input:focus{
      border-color:rgba(15,157,116,.35);
      box-shadow:0 0 0 4px rgba(15,157,116,.10);
      transform:translateY(-1px);
    }

    .hint,.error-msg{
      font-size:.88rem;
      margin-top:8px;
    }

    .hint{color:var(--muted)}
    .error-msg{color:var(--danger);font-weight:600;min-height:20px}

    .actions{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      margin-top:18px;
    }

    .btn{
      border:none;
      border-radius:16px;
      padding:13px 18px;
      font-weight:700;
      cursor:pointer;
      transition:.25s ease;
    }

    .btn:hover{transform:translateY(-2px)}
    .btn-primary{
      background:linear-gradient(135deg,var(--primary),var(--primary-2));
      color:#fff;
      box-shadow:0 10px 24px rgba(15,157,116,.22);
    }

    .btn-soft{
      background:#edf8f2;
      color:var(--primary);
    }

    .btn-danger{
      background:#fdecea;
      color:#c0392b;
    }

    .app{
      display:grid;
      grid-template-columns:290px 1fr;
      min-height:100vh;
    }

    .sidebar{
      background:rgba(255,255,255,.74);
      backdrop-filter:blur(14px);
      border-right:1px solid var(--line);
      padding:22px 18px;
      position:sticky;
      top:0;
      height:100vh;
    }

    .sidebar-head{
      display:flex;
      align-items:center;
      gap:12px;
      margin-bottom:24px;
    }

    .sidebar-mark{
      width:48px;
      height:48px;
      border-radius:15px;
      display:grid;
      place-items:center;
      color:#fff;
      background:linear-gradient(135deg,var(--primary),var(--primary-2));
      font-size:22px;
      font-weight:800;
    }

    .sidebar-head h2{font-size:1.35rem}
    .sidebar-head p{font-size:.88rem;color:var(--muted);margin-top:3px}

    .profile-box{
      background:linear-gradient(135deg, rgba(15,157,116,.96), rgba(34,197,139,.88));
      color:#fff;
      border-radius:22px;
      padding:18px;
      box-shadow:var(--shadow);
      margin-bottom:18px;
    }

    .profile-box .small{font-size:.9rem;opacity:.9;margin-top:6px}

    .nav{
      display:flex;
      flex-direction:column;
      gap:10px;
    }

    .nav-btn{
      background:transparent;
      border:none;
      text-align:left;
      padding:13px 14px;
      border-radius:14px;
      color:var(--text);
      font-weight:600;
      cursor:pointer;
      transition:.22s ease;
    }

    .nav-btn:hover,.nav-btn.active{
      background:#e9f7f0;
      color:var(--primary);
    }

    .main{
      padding:22px;
    }

    .topbar{
      display:flex;
      justify-content:space-between;
      gap:16px;
      align-items:center;
      flex-wrap:wrap;
      margin-bottom:18px;
      background:rgba(255,255,255,.72);
      border:1px solid var(--line);
      border-radius:24px;
      padding:18px 20px;
      backdrop-filter:blur(14px);
      box-shadow:var(--shadow);
    }

    .topbar h1{
      font-size:1.8rem;
      margin-bottom:4px;
    }

    .topbar p{
      color:var(--muted);
    }

    .top-actions{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      align-items:center;
    }

    .pill{
      padding:10px 14px;
      border-radius:999px;
      background:#fff;
      border:1px solid var(--line);
      font-weight:600;
      font-size:.9rem;
      box-shadow:0 3px 8px rgba(0,0,0,.03);
    }

    .grid{
      display:grid;
      grid-template-columns:repeat(12,1fr);
      gap:18px;
    }

    .card{
      grid-column:span 12;
      background:rgba(255,255,255,.84);
      backdrop-filter:blur(10px);
      border:1px solid var(--line);
      border-radius:24px;
      padding:20px;
      box-shadow:var(--shadow);
    }

    .col-4{grid-column:span 4}
    .col-5{grid-column:span 5}
    .col-6{grid-column:span 6}
    .col-7{grid-column:span 7}
    .col-8{grid-column:span 8}
    .col-12{grid-column:span 12}

    .card h3{
      font-size:1.1rem;
      margin-bottom:10px;
    }

    .sub{
      color:var(--muted);
      line-height:1.6;
      font-size:.94rem;
      margin-bottom:14px;
    }

    .metrics{
      display:grid;
      grid-template-columns:repeat(4,1fr);
      gap:12px;
    }

    .metric{
      background:linear-gradient(180deg,#ffffff,#f7fcf9);
      border:1px solid var(--line);
      border-radius:18px;
      padding:16px;
    }

    .metric span{
      display:block;
      font-size:.84rem;
      color:var(--muted);
      margin-bottom:8px;
    }

    .metric strong{
      font-family:'Outfit',sans-serif;
      font-size:1.6rem;
      font-weight:700;
    }

    .form-grid{
      display:grid;
      grid-template-columns:repeat(2,1fr);
      gap:14px;
    }

    .chat{
      border:1px solid var(--line);
      border-radius:18px;
      overflow:hidden;
      background:#fff;
      display:flex;
      flex-direction:column;
      min-height:350px;
    }

    .chat-messages{
      flex:1;
      background:#f7fcf9;
      padding:14px;
      overflow:auto;
      max-height:330px;
    }

    .msg{
      max-width:78%;
      padding:12px 14px;
      border-radius:16px;
      margin-bottom:10px;
      line-height:1.5;
      font-size:.93rem;
    }

    .msg.user{
      background:linear-gradient(135deg,var(--primary),var(--primary-2));
      color:#fff;
      margin-left:auto;
      border-bottom-right-radius:4px;
    }

    .msg.recycler{
      background:#e7f0ff;
      color:#1e3a8a;
      border-bottom-left-radius:4px;
    }

    .chat-input{
      display:flex;
      gap:10px;
      padding:12px;
      border-top:1px solid var(--line);
      background:#fff;
    }

    .badge{
      display:inline-block;
      padding:7px 11px;
      border-radius:999px;
      font-size:.82rem;
      font-weight:700;
      margin-bottom:10px;
    }

    .badge-success{background:#dcfce7;color:#166534}
    .badge-info{background:#dbeafe;color:#1d4ed8}
    .badge-warn{background:#fef3c7;color:#92400e}

    .list{
      display:flex;
      flex-direction:column;
      gap:10px;
    }

    .list-item{
      background:#fbfefd;
      border:1px solid var(--line);
      border-radius:16px;
      padding:14px;
    }

    .list-item strong{
      display:block;
      margin-bottom:5px;
    }

    .history-wrap{
      overflow:auto;
      border:1px solid var(--line);
      border-radius:18px;
      background:#fff;
    }

    table{
      width:100%;
      border-collapse:collapse;
      min-width:760px;
    }

    th,td{
      padding:14px;
      border-bottom:1px solid rgba(21,53,44,.08);
      text-align:left;
      font-size:.93rem;
    }

    th{
      background:#f3faf6;
      color:var(--primary);
      font-weight:700;
    }

    #map{
      width:100%;
      height:320px;
      border-radius:18px;
      overflow:hidden;
      border:1px solid var(--line);
    }

    #previewImage{
      width:100%;
      max-height:280px;
      object-fit:cover;
      border-radius:16px;
      border:1px solid var(--line);
      margin-top:14px;
    }

    .view.hidden{display:none}

    @media (max-width:1100px){
      .login-card{grid-template-columns:1fr}
      .app{grid-template-columns:1fr}
      .sidebar{
        position:relative;
        height:auto;
        border-right:none;
        border-bottom:1px solid var(--line);
      }
      .col-4,.col-5,.col-6,.col-7,.col-8{grid-column:span 12}
      .metrics{grid-template-columns:repeat(2,1fr)}
    }

    @media (max-width:700px){
      .login-hero,.login-panel{padding:26px 20px}
      .hero-title{font-size:2.1rem}
      .role-switch,.form-grid,.metrics{grid-template-columns:1fr}
      .chat-input{flex-direction:column}
      .main{padding:14px}
      .topbar h1{font-size:1.4rem}
    }
  </style>
</head>
<body>

  <section class="login-shell" id="loginScreen">
    <div class="login-card">
      <div class="login-hero">
        <div class="brand">
          <div class="brand-mark">♻</div>
          <div class="brand-text">
            <h1>ReCycleHub</h1>
            <p>Smart waste pickup & recycler coordination</p>
          </div>
        </div>

        <h2 class="hero-title">Cleaner pickups. Smarter recycling.</h2>
        <p class="hero-copy">
          Book waste pickup, scan recyclables, track location, and chat with recyclers in one elegant dashboard.
        </p>

        <div class="hero-points">
          <div class="hero-point">Live pickup booking and role-based dashboard</div>
          <div class="hero-point">Map location support with Leaflet integration</div>
          <div class="hero-point">Recycler-only history access and secure recycler login</div>
        </div>
      </div>

      <div class="login-panel">
        <h2 class="panel-title">Welcome back</h2>
        <p class="panel-sub">Choose your role and continue into the ReCycleHub workspace.</p>

        <div class="role-switch">
          <button class="role-btn active" data-role="user">👤 User</button>
          <button class="role-btn" data-role="recycler">🚚 Recycler</button>
        </div>

        <div class="field">
          <label for="nameInput">Full Name</label>
          <input id="nameInput" class="input" type="text" placeholder="Enter your name">
        </div>

        <div class="field">
          <label for="emailInput">Email</label>
          <input id="emailInput" class="input" type="email" placeholder="Enter your email">
        </div>

        <div class="field" id="secretField" style="display:none;">
          <label for="codeInput">Recycler Access Code</label>
          <input id="codeInput" class="input" type="password" placeholder="Enter recycler code">
          <div class="hint">This field is required only for recycler login.</div>
        </div>

        <div class="error-msg" id="loginError"></div>

        <div class="actions">
          <button class="btn btn-primary" id="loginBtn">Login</button>
          <button class="btn btn-soft" id="demoBtn">Fill Demo</button>
        </div>
      </div>
    </div>
  </section>

  <section class="app hidden" id="appScreen">
    <aside class="sidebar">
      <div class="sidebar-head">
        <div class="sidebar-mark">♻</div>
        <div>
          <h2>ReCycleHub</h2>
          <p>Eco operations panel</p>
        </div>
      </div>

      <div class="profile-box">
        <strong id="profileName">User Name</strong>
        <div class="small" id="profileEmail">email@example.com</div>
        <div class="small" id="profileRole">User Access</div>
      </div>

      <div class="nav">
        <button class="nav-btn active" data-view="dashboard">Dashboard</button>
        <button class="nav-btn" data-view="booking">Book Pickup</button>
        <button class="nav-btn" data-view="scan">Waste Scan</button>
        <button class="nav-btn" data-view="chat">Chat</button>
        <button class="nav-btn hidden" id="historyNav" data-view="history">History</button>
        <button class="nav-btn" id="logoutBtn">Logout</button>
      </div>
    </aside>

    <main class="main">
      <div class="topbar">
        <div>
          <h1 id="pageTitle">User Dashboard</h1>
          <p id="pageSubtitle">Manage bookings, scans, and recycler communication.</p>
        </div>
        <div class="top-actions">
          <div class="pill" id="rolePill">User</div>
          <div class="pill" id="notifPill">Notifications: 3</div>
          <button class="btn btn-soft" id="gpsBtn">Use GPS</button>
        </div>
      </div>

      <section class="view" id="view-dashboard">
        <div class="grid">
          <div class="card col-8">
            <h3>Performance overview</h3>
            <p class="sub">Role-aware metrics update automatically depending on whether you are logged in as a user or recycler.</p>
            <div class="metrics" id="metricsBox"></div>
          </div>

          <div class="card col-4">
            <h3>Notifications</h3>
            <div class="list" id="notificationList"></div>
          </div>

          <div class="card col-7">
            <h3>Pickup map</h3>
            <p class="sub">Capture your current location and visualize recycler proximity on the map.[web:116]</p>
            <div id="map"></div>
            <div class="actions" style="margin-top:14px;">
              <button class="btn btn-primary" id="locateBtn">Locate Me</button>
              <button class="btn btn-soft" id="recyclerMarkerBtn">Show Recycler</button>
            </div>
            <p class="sub" id="locationText" style="margin-top:12px;">Location not captured yet.</p>
          </div>

          <div class="card col-5">
            <h3>Pickup flow</h3>
            <div class="list" id="flowList"></div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-booking">
        <div class="grid">
          <div class="card col-7">
            <h3>Book pickup</h3>
            <div class="form-grid">
              <div class="field">
                <label>Full Name</label>
                <input id="bookName" class="input" type="text">
              </div>
              <div class="field">
                <label>Phone</label>
                <input id="bookPhone" class="input" type="tel" placeholder="98XXXXXXXX">
              </div>
              <div class="field">
                <label>Address</label>
                <input id="bookAddress" class="input" type="text" placeholder="House No, Bāshettihalli">
              </div>
              <div class="field">
                <label>Waste Type</label>
                <select id="wasteType" class="input">
                  <option>Plastic</option>
                  <option>Paper</option>
                  <option>Metal</option>
                  <option>Glass</option>
                  <option>E-waste</option>
                  <option>Battery</option>
                  <option>Mixed</option>
                </select>
              </div>
              <div class="field">
                <label>Weight (kg)</label>
                <input id="weightInput" class="input" type="number" value="5">
              </div>
              <div class="field">
                <label>Pickup Slot</label>
                <select id="slotInput" class="input">
                  <option>10:00 AM - 12:00 PM</option>
                  <option selected>02:00 PM - 04:00 PM</option>
                  <option>05:00 PM - 07:00 PM</option>
                </select>
              </div>
              <div class="field">
                <label>Pickup Date</label>
                <input id="dateInput" class="input" type="date">
              </div>
              <div class="field">
                <label>Notes</label>
                <input id="noteInput" class="input" type="text" placeholder="Landmark or note">
              </div>
            </div>
            <div class="actions">
              <button class="btn btn-primary" id="bookBtn">Confirm Pickup</button>
              <button class="btn btn-soft" id="fillLocationBtn">Use GPS Address</button>
            </div>
          </div>

          <div class="card col-5">
            <h3>Booking summary</h3>
            <div id="bookingSummary">
              <span class="badge badge-info">No active booking</span>
              <p class="sub">Your pickup summary will appear here after booking.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-scan">
        <div class="grid">
          <div class="card col-6">
            <h3>Waste image scan</h3>
            <p class="sub">Upload a waste image to simulate in-browser identification. MobileNet is commonly used for browser image classification workflows in TensorFlow.js demos.[web:163][web:6]</p>
            <input id="scanFile" class="input" type="file" accept="image/*">
            <div class="actions">
              <button class="btn btn-primary" id="scanBtn">Scan Waste</button>
            </div>
            <img id="previewImage" class="hidden" alt="Uploaded waste preview">
          </div>

          <div class="card col-6">
            <h3>Scan result</h3>
            <div id="scanResult">
              <span class="badge badge-warn">Waiting for image</span>
              <p class="sub">Upload an image and run the waste scan.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-chat">
        <div class="grid">
          <div class="card col-7">
            <h3>Live chat</h3>
            <div class="chat">
              <div class="chat-messages" id="chatMessages"></div>
              <div class="chat-input">
                <input id="chatInput" class="input" type="text" placeholder="Type a message...">
                <button class="btn btn-primary" id="sendBtn">Send</button>
              </div>
            </div>
          </div>

          <div class="card col-5">
            <h3>Role notes</h3>
            <div id="roleNotes">
              <span class="badge badge-success">Active session</span>
              <p class="sub">Role-based instructions will appear here.</p>
            </div>
          </div>
        </div>
      </section>

      <section class="view hidden" id="view-history">
        <div class="grid">
          <div class="card col-12">
            <h3>Recycler history</h3>
            <p class="sub">This section is visible only for recycler role.</p>
            <div class="actions">
              <button class="btn btn-danger" id="clearHistoryBtn">Clear History</button>
            </div>
            <div class="history-wrap">
              <table>
                <thead>
                  <tr>
                    <th>ID</th>
                    <th>User</th>
                    <th>Waste Type</th>
                    <th>Weight</th>
                    <th>Status</th>
                    <th>Reward</th>
                    <th>Handled By</th>
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
    const SECRET_HASH = btoa("7204326807");

    const state = {
      selectedRole: "user",
      currentRole: "user",
      currentUser: { name: "", email: "" },
      location: null,
      address: "",
      model: null,
      notifications: [
        { title: "Pickup booked", body: "Your request was submitted successfully." },
        { title: "Recycler assigned", body: "A recycler has been assigned to your request." },
        { title: "Reward credited", body: "Wallet updated after completion." }
      ],
      chats: [
        { role: "recycler", text: "Hello, I will reach in 15 minutes.", time: "2:10 PM" },
        { role: "user", text: "Okay, I will keep the waste ready.", time: "2:11 PM" }
      ],
      history: [
        { id: "PK001", user: "Shobith", waste: "Plastic", weight: "5 kg", status: "Completed", reward: "₹120", by: "Recycler A" },
        { id: "PK002", user: "Arun", waste: "E-waste", weight: "2 kg", status: "Completed", reward: "₹220", by: "Recycler A" },
        { id: "PK003", user: "Meghana", waste: "Paper", weight: "3 kg", status: "Scheduled", reward: "₹60", by: "Recycler B" }
      ],
      metrics: {
        user: [
          { label: "Total Pickups", value: "12" },
          { label: "Rewards Earned", value: "₹460" },
          { label: "Waste Scans", value: "9" },
          { label: "Completion Rate", value: "92%" }
        ],
        recycler: [
          { label: "Assigned Pickups", value: "18" },
          { label: "Daily Earnings", value: "₹1250" },
          { label: "Collected Weight", value: "25 kg" },
          { label: "Completion Rate", value: "96%" }
        ]
      },
      flow: [
        "Pickup requested",
        "Recycler assigned",
        "En route for collection",
        "Waste collected",
        "Reward processed"
      ]
    };

    let map, userMarker, recyclerMarker;

    const roleButtons = document.querySelectorAll(".role-btn");
    const secretField = document.getElementById("secretField");
    const loginError = document.getElementById("loginError");
    const loginScreen = document.getElementById("loginScreen");
    const appScreen = document.getElementById("appScreen");
    const historyNav = document.getElementById("historyNav");

    roleButtons.forEach(btn => {
      btn.addEventListener("click", () => {
        state.selectedRole = btn.dataset.role;
        roleButtons.forEach(b => b.classList.remove("active"));
        btn.classList.add("active");
        secretField.style.display = state.selectedRole === "recycler" ? "block" : "none";
        loginError.textContent = "";
      });
    });

    document.getElementById("demoBtn").addEventListener("click", () => {
      document.getElementById("nameInput").value = state.selectedRole === "recycler" ? "Recycler Demo" : "Shobith Gowda D";
      document.getElementById("emailInput").value = state.selectedRole === "recycler" ? "recycler@recyclehub.com" : "shobith@gmail.com";
      document.getElementById("codeInput").value = "";
    });

    document.getElementById("loginBtn").addEventListener("click", () => {
      const name = document.getElementById("nameInput").value.trim();
      const email = document.getElementById("emailInput").value.trim();
      const enteredCode = document.getElementById("codeInput").value.trim();

      if (!name || !email) {
        loginError.textContent = "Please enter your name and email.";
        return;
      }

      if (state.selectedRole === "recycler") {
        if (btoa(enteredCode) !== SECRET_HASH) {
          loginError.textContent = "Invalid recycler access code.";
          return;
        }
      }

      state.currentRole = state.selectedRole;
      state.currentUser = { name, email };

      loginScreen.classList.add("hidden");
      appScreen.classList.remove("hidden");

      updateRoleUI();
      switchView("dashboard");

      if (!map) {
        setTimeout(initMap, 150);
      }
    });

    function updateRoleUI() {
      const role = state.currentRole;
      document.getElementById("profileName").textContent = state.currentUser.name;
      document.getElementById("profileEmail").textContent = state.currentUser.email;
      document.getElementById("profileRole").textContent = role === "recycler" ? "Recycler Access" : "User Access";
      document.getElementById("rolePill").textContent = role === "recycler" ? "Recycler" : "User";
      document.getElementById("pageTitle").textContent = role === "recycler" ? "Recycler Dashboard" : "User Dashboard";
      document.getElementById("pageSubtitle").textContent = role === "recycler"
        ? "Monitor collection activity, booking requests, and recycler-only history."
        : "Manage bookings, scans, live chat, and pickup updates.";

      historyNav.classList.toggle("hidden", role !== "recycler");
      document.getElementById("view-history").classList.toggle("hidden", role !== "recycler");

      renderMetrics();
      renderNotifications();
      renderFlow();
      renderChats();
      renderHistory();
      renderRoleNotes();
    }

    function renderMetrics() {
      const metricsBox = document.getElementById("metricsBox");
      const items = state.metrics[state.currentRole];
      metricsBox.innerHTML = items.map(item => `
        <div class="metric">
          <span>${item.label}</span>
          <strong>${item.value}</strong>
        </div>
      `).join("");
    }

    function renderNotifications() {
      document.getElementById("notifPill").textContent = `Notifications: ${state.notifications.length}`;
      document.getElementById("notificationList").innerHTML = state.notifications.map(n => `
        <div class="list-item">
          <strong>${n.title}</strong>
          <div style="color:var(--muted);font-size:.9rem;">${n.body}</div>
        </div>
      `).join("");
    }

    function renderFlow() {
      document.getElementById("flowList").innerHTML = state.flow.map(step => `
        <div class="list-item">${step}</div>
      `).join("");
    }

    function renderChats() {
      const chatBox = document.getElementById("chatMessages");
      chatBox.innerHTML = state.chats.map(msg => `
        <div class="msg ${msg.role}">
          ${msg.text}
          <div style="margin-top:6px;font-size:.78rem;opacity:.7;">${msg.time}</div>
        </div>
      `).join("");
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function renderRoleNotes() {
      const notes = document.getElementById("roleNotes");
      notes.innerHTML = state.currentRole === "recycler"
        ? `
          <span class="badge badge-info">Recycler Mode</span>
          <p class="sub">You can access booking records, update pickups, view recycler history, and manage collection workflow.</p>
        `
        : `
          <span class="badge badge-success">User Mode</span>
          <p class="sub">You can book pickup, scan waste, chat with recycler, and view live status updates. History is hidden for users.</p>
        `;
    }

    function renderHistory() {
      const body = document.getElementById("historyBody");
      if (state.currentRole !== "recycler") {
        body.innerHTML = "";
        return;
      }

      if (!state.history.length) {
        body.innerHTML = `<tr><td colspan="7" style="text-align:center;color:var(--muted);">No history available</td></tr>`;
        return;
      }

      body.innerHTML = state.history.map(row => `
        <tr>
          <td>${row.id}</td>
          <td>${row.user}</td>
          <td>${row.waste}</td>
          <td>${row.weight}</td>
          <td>${row.status}</td>
          <td>${row.reward}</td>
          <td>${row.by}</td>
        </tr>
      `).join("");
    }

    function switchView(view) {
      if (view === "history" && state.currentRole !== "recycler") return;

      document.querySelectorAll(".view").forEach(v => v.classList.add("hidden"));
      document.getElementById(`view-${view}`).classList.remove("hidden");

      document.querySelectorAll(".nav-btn[data-view]").forEach(btn => {
        btn.classList.toggle("active", btn.dataset.view === view);
      });

      if (view === "history" && map) {
        setTimeout(() => map.invalidateSize(), 200);
      }
    }

    document.querySelectorAll(".nav-btn[data-view]").forEach(btn => {
      btn.addEventListener("click", () => switchView(btn.dataset.view));
    });

    document.getElementById("logoutBtn").addEventListener("click", () => {
      appScreen.classList.add("hidden");
      loginScreen.classList.remove("hidden");
      document.getElementById("codeInput").value = "";
      loginError.textContent = "";
      switchView("dashboard");
    });

    function initMap() {
      map = L.map("map").setView([13.0456, 77.6234], 13);
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: "&copy; OpenStreetMap contributors"
      }).addTo(map);
      userMarker = L.marker([13.0456, 77.6234]).addTo(map).bindPopup("Default user location");
    }

    function setLocation(lat, lng, label) {
      state.location = { lat, lng };
      state.address = label;
      if (userMarker) map.removeLayer(userMarker);
      userMarker = L.marker([lat, lng]).addTo(map).bindPopup(label).openPopup();
      map.setView([lat, lng], 15);
      document.getElementById("locationText").textContent = `Location: ${label} (${lat.toFixed(5)}, ${lng.toFixed(5)})`;
    }

    document.getElementById("gpsBtn").addEventListener("click", locateUser);
    document.getElementById("locateBtn").addEventListener("click", locateUser);

    function locateUser() {
      if (!navigator.geolocation) {
        document.getElementById("locationText").textContent = "Geolocation not supported in this browser.";
        return;
      }

      navigator.geolocation.getCurrentPosition(
        (pos) => {
          const lat = pos.coords.latitude;
          const lng = pos.coords.longitude;
          setLocation(lat, lng, "Current pickup point");
        },
        () => {
          document.getElementById("locationText").textContent = "Location access denied. Showing default location.";
          setLocation(13.0456, 77.6234, "Bāshettihalli default point");
        },
        { enableHighAccuracy: true, timeout: 10000 }
      );
    }

    document.getElementById("recyclerMarkerBtn").addEventListener("click", () => {
      if (!map) return;
      if (recyclerMarker) map.removeLayer(recyclerMarker);
      recyclerMarker = L.marker([13.0556, 77.6334]).addTo(map).bindPopup("Recycler nearby").openPopup();
    });

    document.getElementById("fillLocationBtn").addEventListener("click", () => {
      if (state.address) {
        document.getElementById("bookAddress").value = state.address;
      }
    });

    document.getElementById("bookBtn").addEventListener("click", () => {
      const name = document.getElementById("bookName").value || state.currentUser.name;
      const waste = document.getElementById("wasteType").value;
      const weight = document.getElementById("weightInput").value;
      const slot = document.getElementById("slotInput").value;
      const date = document.getElementById("dateInput").value || "Not selected";
      const reward = estimateReward(waste, weight);

      document.getElementById("bookingSummary").innerHTML = `
        <span class="badge badge-success">Pickup booked</span>
        <p class="sub"><strong>${name}</strong> booked a <strong>${waste}</strong> pickup for <strong>${weight} kg</strong>.</p>
        <p class="sub">Slot: ${slot}<br>Date: ${date}<br>Estimated reward: ${reward}</p>
      `;

      state.notifications.unshift({
        title: "Pickup booked",
        body: `${waste} pickup scheduled for ${slot}.`
      });

      if (state.currentRole === "recycler") {
        state.history.unshift({
          id: "PK" + String(Math.floor(Math.random() * 900 + 100)),
          user: name,
          waste,
          weight: `${weight} kg`,
          status: "Scheduled",
          reward,
          by: "Self / Assigned Recycler"
        });
        renderHistory();
      }

      renderNotifications();
    });

    function estimateReward(type, weight) {
      const w = Number(weight || 0);
      const base = {
        "Plastic": 12,
        "Paper": 8,
        "Metal": 22,
        "Glass": 10,
        "E-waste": 35,
        "Battery": 28,
        "Mixed": 9
      };
      const total = (base[type] || 10) * Math.max(w, 1);
      return `₹${total}`;
    }

    document.getElementById("sendBtn").addEventListener("click", sendChat);
    document.getElementById("chatInput").addEventListener("keypress", (e) => {
      if (e.key === "Enter") sendChat();
    });

    function sendChat() {
      const input = document.getElementById("chatInput");
      const text = input.value.trim();
      if (!text) return;
      state.chats.push({
        role: state.currentRole === "recycler" ? "recycler" : "user",
        text,
        time: new Date().toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" })
      });
      input.value = "";
      renderChats();
    }

    document.getElementById("scanFile").addEventListener("change", (e) => {
      const file = e.target.files[0];
      if (!file) return;
      const img = document.getElementById("previewImage");
      img.src = URL.createObjectURL(file);
      img.classList.remove("hidden");
    });

    document.getElementById("scanBtn").addEventListener("click", async () => {
      const file = document.getElementById("scanFile").files[0];
      const resultBox = document.getElementById("scanResult");

      if (!file) {
        resultBox.innerHTML = `
          <span class="badge badge-warn">No image selected</span>
          <p class="sub">Please upload an image first.</p>
        `;
        return;
      }

      resultBox.innerHTML = `
        <span class="badge badge-info">Scanning...</span>
        <p class="sub">Loading model and analyzing image.</p>
      `;

      try {
        if (!state.model) {
          state.model = await mobilenet.load();
        }

        const img = document.getElementById("previewImage");
        const predictions = await state.model.classify(img);
        const top = predictions[0];
        const mapped = mapPrediction(top.className);

        resultBox.innerHTML = `
          <span class="badge badge-success">Scan complete</span>
          <p class="sub"><strong>Detected:</strong> ${mapped.type}</p>
          <p class="sub"><strong>Model label:</strong> ${top.className}</p>
          <p class="sub"><strong>Confidence:</strong> ${(top.probability * 100).toFixed(1)}%</p>
          <p class="sub"><strong>Estimated reward:</strong> ${mapped.reward}</p>
        `;
      } catch (err) {
        const fallback = randomFallback();
        resultBox.innerHTML = `
          <span class="badge badge-warn">Fallback result</span>
          <p class="sub"><strong>Detected:</strong> ${fallback.type}</p>
          <p class="sub"><strong>Confidence:</strong> ${fallback.confidence}</p>
          <p class="sub"><strong>Estimated reward:</strong> ${fallback.reward}</p>
        `;
      }
    });

    function mapPrediction(className) {
      const label = className.toLowerCase();
      if (label.includes("bottle") || label.includes("plastic")) return { type: "Plastic", reward: "₹40 - ₹120" };
      if (label.includes("phone") || label.includes("electronics")) return { type: "E-waste", reward: "₹120 - ₹500" };
      if (label.includes("battery")) return { type: "Battery", reward: "₹70 - ₹250" };
      if (label.includes("can") || label.includes("metal")) return { type: "Metal", reward: "₹60 - ₹180" };
      return { type: "Mixed", reward: "₹20 - ₹100" };
    }

    function randomFallback() {
      const items = [
        { type: "Plastic", confidence: "88.4%", reward: "₹40 - ₹120" },
        { type: "Paper", confidence: "84.2%", reward: "₹20 - ₹80" },
        { type: "Metal", confidence: "81.7%", reward: "₹60 - ₹180" },
        { type: "E-waste", confidence: "86.5%", reward: "₹120 - ₹500" }
      ];
      return items[Math.floor(Math.random() * items.length)];
    }

    document.getElementById("clearHistoryBtn").addEventListener("click", () => {
      if (state.currentRole !== "recycler") return;
      state.history = [];
      renderHistory();
    });

    document.getElementById("bookName").value = "Shobith Gowda D";
    document.getElementById("bookPhone").value = "9876543210";
    document.getElementById("bookAddress").value = "Bāshettihalli, Karnataka";
  </script>
</body>
</html>
