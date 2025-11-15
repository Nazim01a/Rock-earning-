<ROCK><html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Rock Earn — Premium Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
:root{--accent:#00f7ef;--dark:#0e0e15;--card:rgba(0,0,0,0.7)}
html,body{height:100%;margin:0;font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;color:#e6f7ff;background:linear-gradient(180deg,#071026 0%, #081423 100%);-webkit-font-smoothing:antialiased;}
canvas#bgCanvas{position:fixed;inset:0;width:100%;height:100%;z-index:-1;}
.sidebar{position:fixed;left:0;top:0;bottom:0;width:84px;background:rgba(10,10,12,0.9);display:flex;flex-direction:column;align-items:center;padding:18px 8px;gap:14px;box-shadow:2px 0 20px rgba(0,0,0,0.6);z-index:50;display:none;}
.logo{font-size:22px}
.icon-btn{width:64px;height:64px;border-radius:14px;background:linear-gradient(180deg,#0b0b10,#0f1220);display:flex;align-items:center;justify-content:center;flex-direction:column;color:#bcdbe3;cursor:pointer;box-shadow:0 6px 18px rgba(0,0,0,0.6);transition:transform .18s,box-shadow .18s;}
.icon-btn i{font-size:18px}
.icon-name{font-size:10px;margin-top:6px;color:#9fd9e9;text-align:center}
.icon-btn:hover{transform:translateY(-6px);box-shadow:0 12px 30px rgba(0,247,239,0.12),0 6px 20px rgba(0,0,0,0.6);}
#welcomeBox{position:fixed;left:100px;right:20px;top:18px;padding:18px;border-radius:14px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));backdrop-filter:blur(8px);box-shadow:0 4px 30px rgba(0,0,0,0.5);display:none;z-index:40;}
#welcomeBox h2{margin:0;font-size:20px;letter-spacing:0.2px}
.stats{display:flex;gap:18px;margin-top:12px;align-items:center;flex-wrap:wrap}
.stat-card{background:linear-gradient(180deg, rgba(0,247,239,0.03), rgba(0,247,239,0.01));padding:12px 16px;border-radius:10px;box-shadow:inset 0 -1px 0 rgba(255,255,255,0.02);min-width:120px}
.stat-label{font-size:11px;color:var(--accent);margin:0 0 6px}
.stat-value{font-size:16px;font-weight:700;margin:0}
.btn{background:transparent;border:1px solid rgba(0,247,239,0.12);padding:10px 12px;border-radius:10px;color:var(--accent);cursor:pointer;font-weight:700;min-width:120px}
.btn:hover{background:rgba(0,247,239,0.03);box-shadow:0 8px 28px rgba(0,247,239,0.03)}
#contentSection{position:fixed;left:100px;top:110px;right:20px;bottom:24px;background:linear-gradient(180deg, rgba(0,0,0,0.45), rgba(0,0,0,0.55));border-radius:14px;padding:18px;overflow:auto;display:none;z-index:30;box-shadow:0 8px 40px rgba(0,0,0,0.6)}
#backBtn{position:absolute;left:12px;top:10px;background:#ff4766;color:white;padding:8px 10px;border-radius:8px;border:none;cursor:pointer;display:inline-block;box-shadow:0 6px 20px rgba(255,71,102,0.12)}
#notif{position:fixed;top:18px;right:-420px;background:linear-gradient(45deg,var(--accent),#9ff0ee);color:#001;padding:12px 20px;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.4);font-weight:700;transition:right .45s;z-index:60}
#notif.show{right:20px}
#authBox{position:fixed;left:120px;top:140px;width:320px;padding:18px;border-radius:12px;background:rgba(0,0,0,0.6);z-index:45;display:block}
#authBox input{width:100%;padding:10px;margin-bottom:10px;border-radius:10px;background:#0f1320;color:#dff7fb;border:1px solid rgba(255,255,255,0.03)}
.plan-card{border-radius:10px;padding:12px;border:1px solid rgba(0,247,239,0.06);margin-bottom:12px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.00));}
.plan-header{display:flex;justify-content:space-between;align-items:center}
.muted{color:#9fcfda;font-size:13px}
.small{font-size:13px}
footer.company{position:fixed;right:18px;bottom:18px;background:rgba(0,0,0,0.6);padding:12px;border-radius:12px;color:#9ff;font-size:13px;box-shadow:0 8px 30px rgba(0,0,0,0.5)}
@media (max-width:900px){ .sidebar{display:flex;bottom:auto;top:0;left:0;right:0;height:auto;width:100%;flex-direction:row;padding:10px} #welcomeBox{left:12px;right:12px} #authBox{left:12px;top:200px} #contentSection{left:12px;top:300px;right:12px;bottom:12px} }
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>
<div class="sidebar" id="sidebar">
<div class="logo">🚀</div>
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><div class="icon-name">Plans</div></div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><div class="icon-name">Deposit</div></div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><div class="icon-name">Withdraw</div></div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><div class="icon-name">Profit</div></div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><div class="icon-name">History</div></div>
<div class="icon-btn" onclick="openSection('support')"><i class="fa fa-headset"></i><div class="icon-name">Support</div></div>
<div class="icon-btn" onclick="openSection('referral')"><i class="fa fa-share-alt"></i><div class="icon-name">Referral</div></div>
<div class="icon-btn" onclick="openSection('share')"><i class="fa fa-share"></i><div class="icon-name">Share</div></div>
</div>
<div id="welcomeBox">
<h2 id="welcomeTxt">🎉 Welcome to <strong style="color:#fff">Rock Earn</strong> Premium 💎</h2>
<div class="stats">
<div class="stat-card"><div class="stat-label">Balance</div><div class="stat-value" id="balValue">0 PKR</div></div>
<div class="stat-card"><div class="stat-label">Total Profit</div><div class="stat-value" id="profValue">0 PKR</div></div>
<div style="display:flex;align-items:center"><button class="btn" onclick="refreshBalance()">Refresh</button></div>
</div>
</div>
<div id="contentSection">
<button id="backBtn" onclick="closeSection()" style="display:none">← Back</button>
<div id="sectionContent"></div>
</div>
<div id="authBox">
<h3 style="margin:0 0 12px 0;text-align:center">Login / Sign Up</h3>
<input id="authName" placeholder="Full Name" />
<input id="authEmail" placeholder="Email (use admin@rockearn.com for admin)" />
<input id="authPass" type="password" placeholder="Password" />
<div style="display:flex;gap:8px">
<button class="btn" onclick="signupUser()"><i class="fa fa-user-plus"></i>&nbsp;Sign Up</button>
<button class="btn" style="background:#00b37e;color:#001;border:none" onclick="loginUser()"><i class="fa fa-sign-in-alt"></i>&nbsp;Login</button>
</div>
<p class="muted small" style="margin-top:10px">Default admin: <strong>admin@rockearn.com</strong> (will be created automatically if not present).</p>
</div>
<button class="logout-btn" id="logoutBtn" onclick="logoutUser()">Logout</button>
<button class="admin-btn" id="adminBtn" onclick="openSection('admin')" title="Admin Panel">Admin Panel</button>
<footer class="company">
<div><strong>Rock Earn Pro LTD</strong></div>
<div style="font-size:13px">25 Main St, California, United States</div>
<div style="font-size:12px;margin-top:6px;color:#9ff">Secure. Fast. Premium support: support@rockearnpro.com</div>
</footer>
<script>
// -----------------------------
// Fully fixed login/signup flow
// -----------------------------
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;
if(!users.find(u => u.email==='admin@rockearn.com')){
  users.push({name:'Administrator', email:'admin@rockearn.com', pass:'admin123', role:'admin', plans:[], deposits:[], withdrawals:[], balance:0, profit:0});
  localStorage.setItem('reUsers', JSON.stringify(users));
}
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }
function signupUser(){ const n=document.getElementById('authName').value.trim(); const e=document.getElementById('authEmail').value.trim().toLowerCase(); const p=document.getElementById('authPass').value; if(!n||!e||!p){showNotif('Fill all fields'); return;} if(!/\S+@\S+\.\S+/.test(e)){showNotif('Invalid email'); return;} if(users.find(u=>u.email===e)){showNotif('Email already registered'); return;} const role=(e==='admin@rockearn.com')?'admin':'user'; const newUser={name:n,email:e,pass:p,role:role,plans:[],deposits:[],withdrawals:[],balance:0,profit:0}; users.push(newUser); localStorage.setItem('reUsers',JSON.stringify(users)); currentUser=newUser; localStorage.setItem('reCurrent',JSON.stringify(currentUser)); openDashboard(); showNotif('Account created & logged in'); }
function loginUser(){ const e=document.getElementById('authEmail').value.trim().toLowerCase(); const p=document.getElementById('authPass').value; const user=users.find(u=>u.email===e && u.pass===p); if(!user){showNotif('Invalid credentials'); return;} currentUser=user; localStorage.setItem('reCurrent',JSON.stringify(currentUser)); openDashboard(); showNotif('Welcome back, '+currentUser.name.split(' ')[0]); }
function logoutUser(){ currentUser=null; localStorage.removeItem('reCurrent'); location.reload(); }
function openDashboard(){ document.getElementById('authBox').style.display='none'; document.getElementById('sidebar').style.display='flex'; document.getElementById('welcomeBox').style.display='block'; document.getElementById('logoutBtn').style.display='block'; document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR'; document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR'; document.getElementById('adminBtn').style.display=(currentUser.role==='admin')?'block':'none'; document.getElementById('backBtn').style.display='none'; }
</script>
</body>
</html>
