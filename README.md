<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{font-family:'Segoe UI',sans-serif;color:white;margin:0;overflow-x:hidden;height:100vh;background:#0e0e15;}
canvas#bgCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}

/* Sidebars */
.sidebar{position:fixed;top:0;left:0;width:140px;height:100vh;background:rgba(20,20,20,0.85);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;opacity:0;transition:1s;}
.sidebar.active{opacity:1;}
.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:#111;color:#ccc;box-shadow:0 0 5px #000;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 15px #0ff,0 0 25px #0ff;}
.icon-name{margin-top:6px;font-size:12px;color:#ccc;text-shadow:0 0 3px #000;}

.sidebar-right{position:fixed;top:20px;right:20px;display:flex;flex-direction:column;gap:15px;z-index:10;}
.icon-btn-right{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:#111;color:#ccc;box-shadow:0 0 5px #000;}
.icon-btn-right:hover{transform:scale(1.1);box-shadow:0 0 15px #0ff,0 0 25px #0ff;}
.icon-name-right{margin-top:6px;font-size:12px;color:#ccc;text-shadow:0 0 3px #000;}

/* Cards / Buttons */
.card{background:rgba(255,255,255,0.05);border-radius:20px;padding:20px;backdrop-filter:blur(15px);box-shadow:0 0 20px #0ff6;transition:0.3s;}
.btn{background:linear-gradient(45deg,#0c0c0c,#222);color:#0ff;padding:12px 18px;border-radius:12px;font-weight:700;transition:0.3s;box-shadow:0 0 5px #0ff,0 0 10px #0ff;cursor:pointer;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.04);}

/* Welcome Box */
@keyframes subtleGlow{0%{text-shadow:0 0 3px #0ff,0 0 6px #0ff;}50%{text-shadow:0 0 8px #0ff,0 0 16px #0ff;}100%{text-shadow:0 0 3px #0ff,0 0 6px #0ff;}}
.stat-card {background: rgba(0,255,255,0.05); padding: 18px 25px; border-radius: 15px; box-shadow: 0 0 5px #0ff, 0 0 10px #0ff; transition: 0.4s; min-width:140px; cursor:default;}
.stat-card:hover {transform: scale(1.05); box-shadow: 0 0 15px #0ff, 0 0 30px #00f;}
.stat-label {font-size:12px;color:#0ff;letter-spacing:1px;margin-bottom:5px;}
.stat-value {font-size:18px;font-weight:700;color:white;}

/* Content Section */
#contentSection{position:fixed;top:20px;left:160px;right:160px;bottom:20px;background:rgba(0,0,0,0.85);backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#0ff,#0ffcc0);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>

<!-- Left Sidebar -->
<div class="sidebar" id="sidebar">
<div style="font-size:28px;margin-bottom:10px;">🚀</div>
<div class="icon-btn" onclick="openSection('home')"><i class="fa fa-home"></i><span class="icon-name">Dashboard</span></div>
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><span class="icon-name">Plans</span></div>
<div class="icon-btn" onclick="openSection('wallet')"><i class="fa fa-wallet"></i><span class="icon-name">Wallet</span></div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><span class="icon-name">Deposit</span></div>
</div>

<!-- Right Sidebar -->
<div class="sidebar-right">
<div class="icon-btn-right" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><span class="icon-name-right">Withdraw</span></div>
<div class="icon-btn-right" onclick="openSection('profit')"><i class="fa fa-coins"></i><span class="icon-name-right">Profit</span></div>
<div class="icon-btn-right" onclick="openSection('history')"><i class="fa fa-clock"></i><span class="icon-name-right">History</span></div>
<div class="icon-btn-right" onclick="openSection('support')"><i class="fa fa-headset"></i><span class="icon-name-right">Support</span></div>
<div class="icon-btn-right" onclick="openSection('share')"><i class="fa fa-share-alt"></i><span class="icon-name-right">Referral</span></div>
</div>

<!-- Auth Box -->
<div id="authBox" class="card" style="margin-left:160px;margin-top:20px;width:320px;">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" />
<input id="authEmail" placeholder="Email" />
<input id="authPass" type="password" placeholder="Password" />
<button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
<button onclick="loginUser()" class="btn w-full mb-2 bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>

<!-- Welcome Box -->
<div id="welcomeBox" style="display:none;margin-left:160px;" class="card mb-4 p-4 text-center"></div>

<!-- Content Section -->
<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<button onclick="logoutUser()" class="btn bg-red-600" style="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);width:200px;">Logout</button>

<script>
// BG Animation
const canvas=document.getElementById('bgCanvas');const ctx=canvas.getContext('2d');canvas.width=window.innerWidth;canvas.height=window.innerHeight;let particles=[];for(let i=0;i<150;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+1,s:Math.random()*0.5+0.5,color:`rgba(0,255,255,${Math.random()})`});}function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle=p.color;ctx.fill();p.y-=p.s;if(p.y<0)p.y=canvas.height;});requestAnimationFrame(animateParticles);}animateParticles();

// Notifications
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg;n.classList.add('show');setTimeout(()=>{n.classList.remove('show');},3000);}

// Users/Auth
let users=JSON.parse(localStorage.getItem('reUsers'))||[];let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
window.addEventListener('load',()=>{if(currentUser){openDashboard();document.getElementById('sidebar').classList.add('active');}});

function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return showNotif('Fill all fields');if(!validateEmail(e))return showNotif('Invalid email');if(users.find(u=>u.email===e))return showNotif('Email already registered');let u={name:n,email:e,pass:p,plans:[],balance:0,profit:0};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return showNotif('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}

// Dashboard Welcome
function openDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('sidebar').classList.add('active');
  document.getElementById('welcomeBox').style.display='block';
  document.getElementById('welcomeBox').innerHTML=`
  <h2 style="font-size:28px;font-weight:700;color:#0ff;letter-spacing:1px;animation: subtleGlow 2s ease-in-out infinite alternate;">
    Welcome to <span style="color:#0f0;font-weight:800;">Rock Earn</span> Premium
  </h2>
  <div style="display:flex;justify-content:center;gap:20px;margin-top:20px;flex-wrap:wrap;">
    <div class="stat-card">
      <p class="stat-label">Balance</p>
      <p class="stat-value">${currentUser.balance} PKR</p>
    </div>
    <div class="stat-card">
      <p class="stat-label">Total Profit</p>
      <p class="stat-value">${currentUser.profit} PKR</p>
    </div>
  </div>`;
}

function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function refreshBalance(){openDashboard();showNotif('Balance refreshed!');}
function closeSection(){document.getElementById('contentSection').style.display='none';}
</script>
</body>
</html>
