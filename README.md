<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{font-family:'Segoe UI',sans-serif;margin:0;padding:0;background:linear-gradient(120deg,#1b1b1b,#111);color:white;overflow-x:hidden;}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@keyframes fadeInLeft{from{opacity:0;transform:translateX(-50px);}to{opacity:1;transform:translateX(0);}}
@keyframes fadeInDown{from{opacity:0;transform:translateY(-50px);}to{opacity:1;transform:translateY(0);}}
@keyframes fadeInUp{from{opacity:0;transform:translateY(50px);}to{opacity:1;transform:translateY(0);}}
@keyframes glow{0%{text-shadow:0 0 5px #00bfff,0 0 10px #00bfff,0 0 20px #00bfff;}50%{text-shadow:0 0 10px #00ffff,0 0 20px #00ffff,0 0 30px #00ffff;}100%{text-shadow:0 0 5px #00bfff,0 0 10px #00bfff,0 0 20px #00bfff;}}
@keyframes bannerAnim{0%{background:linear-gradient(90deg,#00bfff,#00ffff);}50%{background:linear-gradient(90deg,#00ffff,#00bfff);}100%{background:linear-gradient(90deg,#00bfff,#00ffff);}}
#banner{position:fixed;top:0;left:0;width:100%;height:100px;background:linear-gradient(90deg,#00bfff,#00ffff);display:flex;align-items:center;justify-content:center;animation:bannerAnim 5s infinite alternate;z-index:999;font-size:32px;font-weight:bold;color:#fff;text-shadow:0 0 10px #00ffff;}
#authBox{display:flex;flex-direction:column;justify-content:center;align-items:center;height:100vh;animation:fadeIn 1s;}
input,button{padding:10px;margin:5px;border-radius:8px;border:none;}
button{background:#00bfff;color:white;cursor:pointer;transition:0.3s;}
button:hover{background:#009acd;transform:scale(1.05);}
#notif{position:fixed;top:20px;right:20px;background:#00bfff;padding:10px 20px;border-radius:8px;opacity:0;transition:0.3s;z-index:1000;}
#notif.show{opacity:1;}
#sidebar{display:none;position:fixed;left:0;top:0;width:250px;height:100vh;background:#111;flex-direction:column;padding:10px;gap:10px;animation:fadeInLeft 1s;}
#sidebar button{width:100%;text-align:left;transition:0.3s;}
#sidebar button:hover{transform:scale(1.05);}
#welcomeBox{display:none;position:fixed;top:120px;left:260px;animation:fadeInDown 1s;}
#userNameTxt{animation:glow 2s infinite;}
#contentSection{margin-left:260px;margin-top:200px;padding:20px;animation:fadeInUp 1s;}
.plan-card{background:#222;margin:10px;padding:15px;border-radius:10px;transition:0.3s;}
.plan-card:hover{background:#333;transform:scale(1.05);box-shadow:0 0 20px #00bfff;}
#profileModal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;animation:fadeIn 0.5s;}
</style>
</head>
<body>
<div id="banner">💎 Rock Earn 💎</div>
<div id="notif"></div>

<!-- Auth -->
<div id="authBox">
<h1 class="text-3xl font-bold mb-4" style="animation:glow 2s infinite;">Welcome to <span style="color:#00bfff">Rock Earn</span></h1>
<input type="text" id="authName" placeholder="Your Name (Signup only)">
<input type="email" id="authEmail" placeholder="Email">
<input type="password" id="authPass" placeholder="Password">
<div>
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>
</div>

<!-- Sidebar -->
<div id="sidebar">
<button onclick="openSection('plans')"><i class="fas fa-list"></i> Plans</button>
<button onclick="openSection('deposit')"><i class="fas fa-wallet"></i> Deposit</button>
<button onclick="openSection('withdraw')"><i class="fas fa-money-bill-wave"></i> Withdraw</button>
<button onclick="openSection('profit')"><i class="fas fa-chart-line"></i> Profit</button>
<button onclick="openSection('history')"><i class="fas fa-history"></i> History</button>
<button id="adminBtn" style="display:none;" onclick="openSection('admin')"><i class="fas fa-user-shield"></i> Admin</button>
<button onclick="openProfile()"><i class="fas fa-user"></i> Profile</button>
<button onclick="logoutUser()"><i class="fas fa-sign-out-alt"></i> Logout</button>
<button id="refreshBtn" onclick="refreshDashboard()">&#x21bb; Refresh</button>
</div>

<!-- Welcome -->
<div id="welcomeBox">
<h2 id="userNameTxt" style="font-size:24px;font-weight:bold;color:#00bfff;"></h2>
<p>Balance: <span id="balValue">0</span> | Profit: <span id="profValue">0</span></p>
</div>

<!-- Content -->
<div id="contentSection"></div>

<!-- Profile Modal -->
<div id="profileModal">
<div style="background:#111;padding:20px;border-radius:10px;display:flex;flex-direction:column;">
<h3>Edit Profile</h3>
<input type="text" id="profileName" placeholder="Name">
<input type="email" id="profileEmail" placeholder="Email">
<input type="password" id="profilePass" placeholder="New Password">
<div>
<button onclick="updateProfile()">Update</button>
<button onclick="closeProfile()">Close</button>
</div>
</div>
</div>

<script>
// Users & Storage
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
    users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
    localStorage.setItem('reUsers',JSON.stringify(users));
}

// Plans
const plans=[{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},{name:'Advanced',amount:1500,daily:250,days:25},{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30},{name:'Platinum',amount:5000,daily:950,days:40},{name:'Diamond',amount:7000,daily:1350,days:50},{name:'VIP',amount:10000,daily:2000,days:60},{name:'Elite',amount:12000,daily:2400,days:60},{name:'Master',amount:15000,daily:3000,days:70},{name:'Ultimate',amount:18000,daily:3500,days:75},{name:'Legend',amount:20000,daily:4000,days:80},{name:'Supreme',amount:25000,daily:5000,days:90},{name:'Titan',amount:30000,daily:6000,days:100}];

// Notifications
function showNotif(msg){const el=document.getElementById('notif');el.innerText=msg;el.classList.add('show');setTimeout(()=>el.classList.remove('show'),3000);}

// Auth Functions
function signupUser(){
    const n=document.getElementById('authName').value.trim();
    const e=document.getElementById('authEmail').value.trim().toLowerCase();
    const p=document.getElementById('authPass').value;
    if(!n||!e||!p){showNotif('Fill all fields');return;}
    if(users.find(u=>u.email===e)){showNotif('Email exists');return;}
    let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
    users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));
    currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard();showNotif('Account created & logged in');
}
function loginUser(){
    const e=document.getElementById('authEmail').value.trim().toLowerCase();
    const p=document.getElementById('authPass').value;
    const u=users.find(x=>x.email===e&&x.pass===p);
    if(!u){showNotif('Invalid credentials');return;}
    currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard();showNotif('Welcome '+currentUser.name.split(' ')[0]);
}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function refreshDashboard(){if(currentUser){currentUser=users.find(u=>u.email===currentUser.email);localStorage.setItem('reCurrent',JSON.stringify(currentUser));document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';showNotif('Dashboard refreshed');}}

// Dashboard
function openDashboard(){
    document.getElementById('authBox').style.display='none';
    document.getElementById('sidebar').style.display='flex';
    document.getElementById('welcomeBox').style.display='block';
    document.getElementById('refreshBtn').style.display='inline-block';
    document.getElementById('userNameTxt').innerText=currentUser.name.split(' ')[0];
    document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
    document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
    if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
    document.getElementById('contentSection').style.display='block';
}

// Init
if(currentUser) openDashboard();
</script>
</body>
</html>
