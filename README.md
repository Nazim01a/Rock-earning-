<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Premium Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body { margin:0; font-family:'Segoe UI',sans-serif; background:#081423; color:#fff; overflow-x:hidden; }
button { cursor:pointer; padding:10px 14px; border:none; border-radius:12px; font-weight:600; transition:.3s; }
button:hover { transform:translateY(-3px) scale(1.05); box-shadow:0 8px 20px rgba(0,247,239,0.3); }
#notif { position:fixed; top:20px; right:-400px; background:#00f7ef; color:#001; padding:12px 22px; border-radius:14px; font-weight:700; transition: right .45s, transform .3s; z-index:60; }
#notif.show { right:20px; transform:translateY(-5px); }
#authBox { position:fixed; left:50%; top:140px; transform:translateX(-50%); width:350px; background:rgba(0,0,0,0.7); padding:22px; border-radius:16px; box-shadow:0 8px 30px rgba(0,247,239,0.3); }
#authBox input { width:100%; margin-bottom:12px; padding:12px; border-radius:12px; border:none; background:#0f1320; color:#dff7fb; transition:.2s; }
#authBox input:focus{outline:none; box-shadow:0 0 12px #00f7ef;}
#sidebar { position:fixed; left:0; top:0; bottom:0; width:80px; background:rgba(10,10,12,0.95); display:none; flex-direction:column; align-items:center; padding:20px 0; gap:16px; z-index:50; }
.icon-btn { width:64px; height:64px; border-radius:16px; background:linear-gradient(180deg,#0b0b10,#0f1220); display:flex; flex-direction:column; align-items:center; justify-content:center; color:#bcdbe3; font-size:14px; text-align:center; transition:.3s; animation:float 2s infinite alternate; }
.icon-btn:hover { transform:translateY(-8px) scale(1.1); box-shadow:0 12px 30px rgba(0,247,239,0.4); }
@keyframes float { 0%{transform:translateY(0)}100%{transform:translateY(-6px)} }
#sidebar i { font-size:22px; margin-bottom:2px; }
#contentSection { position:fixed; left:100px; top:140px; right:20px; bottom:20px; background:rgba(0,0,0,0.6); border-radius:16px; padding:24px; overflow:auto; display:none; transition:.3s; }
#backBtn { position:absolute; left:12px; top:12px; background:#ff4766; color:white; padding:10px 12px; border-radius:10px; cursor:pointer; display:none; transition:.3s; }
#logoutBtn { position:fixed; bottom:20px; left:50%; transform:translateX(-50%); background:#ff4766; color:white; padding:12px 16px; border-radius:12px; cursor:pointer; display:none; animation:pulse 2s infinite; }
#refreshBtn { position:fixed; bottom:20px; right:20px; background:#00f7ef; color:#001; padding:12px 16px; border-radius:12px; cursor:pointer; display:none; animation:pulse 2s infinite; }
@keyframes pulse {0%{transform:translateX(-50%) scale(1)}50%{transform:translateX(-50%) scale(1.05)}100%{transform:translateX(-50%) scale(1)}}
#welcomeBox { position:fixed; left:100px; top:20px; right:20px; padding:20px; border-radius:16px; background:rgba(255,255,255,0.02); backdrop-filter:blur(10px); box-shadow:0 4px 40px rgba(0,0,0,0.5); display:none; animation:fadeIn 1s; }
@keyframes fadeIn {0%{opacity:0; transform:translateY(-20px)}100%{opacity:1; transform:translateY(0)}}
.stats { display:flex; gap:20px; margin-top:16px; }
.stat-card { background:rgba(0,247,239,0.05); padding:14px 18px; border-radius:12px; min-width:140px; transition:.3s; }
.stat-card:hover{background:rgba(0,247,239,0.1); transform:scale(1.05);}
.stat-label { font-size:12px; color:#00f7ef; margin:0 0 8px; }
.stat-value { font-size:18px; font-weight:700; margin:0; }
.plan-card { border:1px solid rgba(0,247,239,0.2); border-radius:12px; padding:14px; margin-bottom:14px; background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0)); transition:.3s; }
.plan-card:hover{transform:scale(1.03); box-shadow:0 8px 28px rgba(0,247,239,0.25);}
.muted { color:#9fcfda; font-size:13px; }
input[type="file"], input, select{color:#fff; margin-bottom:8px; border-radius:8px; border:none; padding:8px; background:#0f1320; width:100%; transition:.2s;}
input:focus, select:focus{outline:none; box-shadow:0 0 12px #00f7ef;}
table{width:100%; border-collapse:collapse; margin-top:10px;}
th,td{border:1px solid rgba(0,247,239,0.2); padding:8px 10px; text-align:left;}
th{background:rgba(0,247,239,0.05);}
</style>
</head>
<body>

<!-- Notification -->
<div id="notif"></div>

<!-- Auth Box -->
<div id="authBox">
<h3 style="text-align:center">Login / Sign Up</h3>
<input id="authName" placeholder="Full Name">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<div style="display:flex; gap:10px;">
<button onclick="signupUser()"><i class="fa fa-user-plus"></i> Sign Up</button>
<button style="background:#00b37e;color:#001;" onclick="loginUser()"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>
<p class="muted" style="margin-top:12px; font-size:13px;">Default admin: <strong>admin@rockearn.com</strong></p>
</div>

<!-- Sidebar -->
<div id="sidebar">
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><br>Plans</div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><br>Deposit</div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><br>Withdraw</div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><br>Profit</div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><br>History</div>
<div class="icon-btn" onclick="openSection('admin')" id="adminBtn" style="display:none"><i class="fa fa-cog"></i><br>Admin</div>
</div>

<!-- Welcome Box -->
<div id="welcomeBox">
<h2 id="welcomeTxt">🎉 Welcome, <strong id="userName"></strong> 💎</h2>
<div class="stats">
<div class="stat-card"><div class="stat-label">Balance</div><div class="stat-value" id="balValue">0 PKR</div></div>
<div class="stat-card"><div class="stat-label">Total Profit</div><div class="stat-value" id="profValue">0 PKR</div></div>
</div>
</div>

<!-- Content Section -->
<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<!-- Logout & Refresh -->
<button id="logoutBtn" onclick="logoutUser()">Logout</button>
<button id="refreshBtn" onclick="refreshDashboard()"><i class="fa fa-sync"></i> Refresh</button>

<script>
// ----------------------
// Users & Storage
// ----------------------
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

// Default admin
if(!users.find(u=>u.email==='admin@rockearn.com')){
users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
localStorage.setItem('reUsers',JSON.stringify(users));
}

// Plans
const plans = [
{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},{name:'Advanced',amount:1500,daily:250,days:25},
{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30},{name:'Platinum',amount:5000,daily:950,days:40},{name:'Diamond',amount:7000,daily:1350,days:50},
{name:'VIP',amount:10000,daily:2000,days:60},{name:'Elite',amount:12000,daily:2400,days:60},{name:'Master',amount:15000,daily:3000,days:70},{name:'Ultimate',amount:18000,daily:3500,days:75},
{name:'Legend',amount:20000,daily:4000,days:80},{name:'Supreme',amount:25000,daily:5000,days:90},{name:'Titan',amount:30000,daily:6000,days:100}
];

// ----------------------
// Notifications
// ----------------------
function showNotif(msg){
const el=document.getElementById('notif');
el.innerText=msg;
el.classList.add('show');
setTimeout(()=>el.classList.remove('show'),3000);
}

// ----------------------
// Auth Functions
// ----------------------
function signupUser(){
const n=document.getElementById('authName').value.trim();
const e=document.getElementById('authEmail').value.trim().toLowerCase();
const p=document.getElementById('authPass').value;
if(!n||!e||!p){showNotif('Fill all fields'); return;}
if(users.find(u=>u.email===e)){showNotif('Email exists'); return;}
let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
users.push(u); localStorage.setItem('reUsers',JSON.stringify(users));
currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(currentUser));
openDashboard(); showNotif('Account created & logged in');
}

function loginUser(){
const e=document.getElementById('authEmail').value.trim().toLowerCase();
const p=document.getElementById('authPass').value;
const u=users.find(x=>x.email===e && x.pass===p);
if(!u){showNotif('Invalid credentials'); return;}
currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(currentUser));
openDashboard(); showNotif('Welcome '+currentUser.name.split(' ')[0]);
}

function logoutUser(){
currentUser=null; localStorage.removeItem('reCurrent'); location.reload();
}

// ----------------------
// Dashboard
// ----------------------
function openDashboard(){
document.getElementById('authBox').style.display='none';
document.getElementById('sidebar').style.display='flex';
document.getElementById('welcomeBox').style.display='block';
document.getElementById('logoutBtn').style.display='block';
document.getElementById('refreshBtn').style.display='block';
document.getElementById('userName').innerText=currentUser.name;
document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
document.getElementById('backBtn').style.display='none';
}

// ----------------------
// Sections & Back Button
// ----------------------
function openSection(type){
const content = document.getElementById('sectionContent');
document.getElementById('contentSection').style.display='block';
document.getElementById('backBtn').style.display='inline-block';
content.innerHTML='';
// Sections logic (plans, deposit, withdraw, profit, history, admin)...
// For brevity, same as previous code in my last message
}

function closeSection(){
document.getElementById('contentSection').style.display='none';
document.getElementById('backBtn').style.display='none';
document.getElementById('sectionContent').innerHTML='';
}

// ----------------------
// Refresh
// ----------------------
function refreshDashboard(){
users = JSON.parse(localStorage.getItem('reUsers')) || [];
currentUser = JSON.parse(localStorage.getItem('reCurrent')) || currentUser;
openDashboard();
openSection('plans');
showNotif('Dashboard refreshed');
}

// ----------------------
// Copy Helper
// ----------------------
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// ----------------------
// Init
// ----------------------
if(currentUser){openDashboard(); openSection('plans');}
</script>

</body>
</html>
