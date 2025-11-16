<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap');

body{margin:0;padding:0;font-family:'Orbitron',sans-serif;background:#111;color:#fff;overflow-x:hidden;position:relative;}
body::before{
  content:'';
  position:fixed;top:0;left:0;width:100%;height:100%;
  background:linear-gradient(270deg,#111,#222,#000,#111,#222);
  background-size:1200% 1200%;animation:bgAnim 30s ease infinite;
  z-index:-1;
}
@keyframes bgAnim{0%{background-position:0% 50%;}50%{background-position:100% 50%;}100%{background-position:0% 50%;}}

#authBox{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;text-align:center;animation:fadeIn 1s;padding:10px;}
input,button{padding:10px;margin:5px;border-radius:8px;border:none;}
input{width:220px;}
button{background:#00ffff;color:#000;font-weight:bold;cursor:pointer;transition:0.3s;}
button:hover{background:#ff00ff;color:#fff;}

#notif{position:fixed;top:20px;right:20px;background:#00ffff;color:#000;padding:10px 20px;border-radius:8px;opacity:0;transition:0.3s;font-weight:bold;z-index:50;}
#notif.show{opacity:1;}

#sidebar{
  display:none;position:fixed;left:0;top:0;width:250px;height:100vh;background:rgba(0,0,0,0.85);
  flex-direction:column;padding:10px;gap:10px;animation:fadeInLeft 1s;overflow-y:auto;z-index:20;
}
#sidebar button{width:100%;text-align:left;background:transparent;border:1px solid #00ffff;color:#00ffff;}
#sidebar button:hover{background:#00ffff;color:#000;}

#welcomeBox{
  display:none;position:fixed;top:20px;left:50%;transform:translateX(-50%);
  flex-direction:row;gap:20px;animation:fadeInDown 1s;z-index:10;
  flex-wrap:wrap;justify-content:center;
}
.neon-card{
  background: rgba(0,255,255,0.1);border:2px solid #00ffff;padding:15px 25px;border-radius:15px;
  text-align:center;width:150px;animation:neonPulse 2s infinite alternate;
  box-shadow:0 0 10px #00ffff,0 0 20px #00ffff,0 0 30px #ff00ff;
}
.neon-card h3{font-size:16px;color:#00ffff;text-shadow:0 0 5px #00ffff,0 0 10px #00ffff,0 0 20px #ff00ff;}
.neon-card p{font-size:18px;font-weight:bold;color:#fff;text-shadow:0 0 5px #00ffff,0 0 10px #00ffff,0 0 20px #ff00ff;}

@keyframes neonPulse{0%{box-shadow:0 0 10px #00ffff,0 0 20px #00ffff,0 0 30px #ff00ff;}50%{box-shadow:0 0 20px #00ffff,0 0 40px #00ffff,0 0 60px #ff00ff;}100%{box-shadow:0 0 10px #00ffff,0 0 20px #00ffff,0 0 30px #ff00ff;}}

#contentSection{margin-left:270px;margin-top:140px;padding:20px;animation:fadeInUp 1s;}
#profileModal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.95);justify-content:center;align-items:center;animation:fadeIn 0.5s;z-index:30;}
#profileModal div{background:#111;padding:20px;border-radius:15px;text-align:center;}

.plan-card{background:#111;margin:10px;padding:15px;border-radius:12px;border:1px solid #00ffff;transition:0.3s;text-align:center;}
.plan-card:hover{transform:scale(1.05);background:#000;border-color:#ff00ff;}

.neon-text{color:#00ffff;text-shadow:0 0 5px #00ffff,0 0 10px #00ffff,0 0 20px #00ffff,0 0 40px #ff00ff,0 0 80px #ff00ff;}

@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@keyframes fadeInLeft{from{opacity:0;transform:translateX(-50px);}to{opacity:1;transform:translateX(0);}}
@keyframes fadeInDown{from{opacity:0;transform:translateY(-50px);}to{opacity:1;transform:translateY(0);}}
@keyframes fadeInUp{from{opacity:0;transform:translateY(50px);}to{opacity:1;transform:translateY(0);}}

/* --- Responsive Fixes --- */
@media(max-width:768px){
  #sidebar{position:fixed;width:200px;left:-210px;transition:0.3s;}
  #sidebar.show{left:0;}
  #contentSection{margin-left:0;margin-top:220px;}
  #welcomeBox{top:10px;flex-direction:column;gap:10px;}
  .neon-card{width:120px;padding:10px;}
  input{width:180px;}
}

.menu-toggle{
  display:none;position:fixed;top:15px;left:15px;background:#00ffff;color:#000;padding:10px;border-radius:8px;cursor:pointer;z-index:40;font-weight:bold;}
.menu-toggle:hover{background:#ff00ff;color:#fff;}
@media(max-width:768px){.menu-toggle{display:block;}}
</style>
</head>
<body>

<div id="notif"></div>

<div class="menu-toggle" onclick="toggleSidebar()"><i class="fas fa-bars"></i> Menu</div>

<div id="authBox">
  <h1 class="text-3xl font-bold mb-4 neon-text">Rock Earn</h1>
  <input type="text" id="authName" placeholder="Your Name (Signup only)">
  <input type="email" id="authEmail" placeholder="Email">
  <input type="password" id="authPass" placeholder="Password">
  <div>
    <button onclick="signupUser()">Sign Up</button>
    <button onclick="loginUser()">Login</button>
  </div>
</div>

<div id="sidebar">
  <button onclick="openSection('plans')"><i class="fas fa-list"></i> Plans</button>
  <button onclick="openSection('deposit')"><i class="fas fa-wallet"></i> Deposit</button>
  <button onclick="openSection('withdraw')"><i class="fas fa-money-bill-wave"></i> Withdraw</button>
  <button onclick="openSection('profit')"><i class="fas fa-chart-line"></i> Profit</button>
  <button onclick="openSection('history')"><i class="fas fa-history"></i> History</button>
  <button id="adminBtn" style="display:none;" onclick="openSection('admin')"><i class="fas fa-user-shield"></i> Admin</button>
  <button onclick="openSection('about')"><i class="fas fa-info-circle"></i> About</button>
  <button onclick="openProfile()"><i class="fas fa-user"></i> Profile</button>
  <button onclick="logoutUser()"><i class="fas fa-sign-out-alt"></i> Logout</button>
</div>

<div id="welcomeBox">
  <div class="neon-card"><h3>Balance</h3><p id="balValue">0 PKR</p></div>
  <div class="neon-card"><h3>Profit</h3><p id="profValue">0 PKR</p></div>
</div>

<div id="contentSection"></div>

<div id="profileModal">
  <div>
    <h3 class="neon-text">Edit Profile</h3>
    <input type="text" id="profileName" placeholder="Name"><br>
    <input type="email" id="profileEmail" placeholder="Email"><br>
    <input type="password" id="profilePass" placeholder="New Password"><br>
    <button onclick="updateProfile()">Update</button>
    <button onclick="closeProfile()">Close</button>
  </div>
</div>

<script>
// --- Data ---
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
  users.push({name:'John Wilson',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
  localStorage.setItem('reUsers',JSON.stringify(users));
}
const plans=[
{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},
{name:'Advanced',amount:1500,daily:250,days:25},{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30}];

// --- Notifications ---
function showNotif(msg){const el=document.getElementById('notif');el.innerText=msg;el.classList.add('show');setTimeout(()=>el.classList.remove('show'),3000);}

// --- Auth ---
function signupUser(){
  const n=document.getElementById('authName').value.trim();
  const e=document.getElementById('authEmail').value.trim().toLowerCase();
  const p=document.getElementById('authPass').value;
  if(!n||!e||!p){showNotif('Fill all fields');return;}
  if(users.find(u=>u.email===e)){showNotif('Email exists');return;}
  let u={name:n,email:e,pass:p,role:'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
  users.push(u); localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  openDashboard(); showNotif('Account created & logged in');
}
function loginUser(){
  const e=document.getElementById('authEmail').value.trim().toLowerCase();
  const p=document.getElementById('authPass').value;
  const u=users.find(x=>x.email===e&&x.pass===p);
  if(!u){showNotif('Invalid credentials');return;}
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  openDashboard(); showNotif('Welcome '+currentUser.name.split(' ')[0]);
}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

// --- Dashboard ---
function openDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('sidebar').style.display='flex';
  document.getElementById('welcomeBox').style.display='flex';
  document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
  document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
  if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
  document.getElementById('contentSection').style.display='block';
  openSection('plans');
}

// --- Sidebar Toggle ---
function toggleSidebar(){
  const sb=document.getElementById('sidebar');
  sb.classList.toggle('show');
}

// --- Responsive Fix End ---
// Deposit / Withdraw / Profile etc functions same as earlier
</script>

</body>
</html>
