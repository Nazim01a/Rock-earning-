<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;color:white;margin:0;overflow-x:hidden;height:100vh;background:#0f0f20;}
canvas#bgCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}

/* Sidebar */
.sidebar{position:fixed;top:0;left:0;width:120px;height:100vh;background:rgba(0,0,0,0.3);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;opacity:0;transition:1s;}
.sidebar.active{opacity:1;}
.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:linear-gradient(45deg,#00f,#0ff);box-shadow:0 0 10px #0ff,0 0 20px #00f;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 20px #0ff,0 0 40px #00f;}
.icon-name{margin-top:6px;font-size:12px;color:#0ff;text-shadow:0 0 5px #0ff;}

/* Sub-icons / dropdown */
.sub-icons{display:none;flex-direction:column;gap:10px;margin-top:5px;}
.sub-icons.active{display:flex;}
.sub-icon{background:linear-gradient(45deg,#0ff,#00f);padding:6px 12px;border-radius:12px;text-align:center;cursor:pointer;transition:0.3s;font-size:12px;}
.sub-icon:hover{transform:scale(1.05);box-shadow:0 0 15px #0ff,0 0 30px #00f;}

/* Cards / Buttons */
.card{background:rgba(255,255,255,0.05);border-radius:20px;padding:20px;backdrop-filter:blur(15px);box-shadow:0 0 30px #0ff6;transition:0.3s;}
.btn{background:linear-gradient(45deg,#00f,#0ff);padding:12px 18px;border-radius:12px;font-weight:700;transition:0.3s;box-shadow:0 0 10px #0ff,0 0 20px #00f;cursor:pointer;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.04);}

/* Welcome Box Neon */
@keyframes neonGlow {from { text-shadow:0 0 5px #0ff,0 0 10px #0ff,0 0 20px #0ff,0 0 30px #00f,0 0 40px #0ff; } to { text-shadow:0 0 10px #0ff,0 0 20px #0ff,0 0 30px #0ff,0 0 40px #00f,0 0 50px #0ff; }}
.stat-card {background: rgba(0,255,255,0.1); padding: 18px 25px; border-radius: 15px; box-shadow: 0 0 10px #0ff, 0 0 20px #0ff; transition: 0.4s; min-width:140px; cursor:default;}
.stat-card:hover {transform: scale(1.05); box-shadow: 0 0 25px #0ff, 0 0 50px #00f;}
.stat-label {font-size:12px;color:#0ff;letter-spacing:1px;margin-bottom:5px;}
.stat-value {font-size:18px;font-weight:700;color:white;}

#contentSection{position:fixed;top:20px;left:140px;right:20px;bottom:20px;background:rgba(0,0,0,0.8);backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#00f,#0ff);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}

/* Video Overlay */
#videoOverlay{position:fixed;top:0;left:0;width:100%;height:100%;background:black;display:flex;justify-content:center;align-items:center;z-index:9999;display:none;}
#videoOverlay video{width:90%;border-radius:15px;box-shadow:0 0 50px #0ff;}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>

<!-- Video Overlay -->
<div id="videoOverlay">
<video id="loginVideo" autoplay muted>
<source src="https://assets.mixkit.co/videos/preview/mixkit-sport-car-driving-on-road-1225-large.mp4" type="video/mp4">
</video>
</div>

<!-- Sidebar -->
<div class="sidebar" id="sidebar">
<div style="font-size:28px;margin-bottom:10px;">🚀</div>
<div class="icon-btn" onclick="openSection('home')"><i class="fa fa-home"></i><span class="icon-name">Dashboard</span></div>
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><span class="icon-name">Plans</span></div>
<div class="icon-btn" onclick="toggleSubIcons()"><i class="fa fa-wallet"></i><span class="icon-name">Finance</span></div>
<div class="sub-icons" id="financeSub">
<div class="sub-icon" onclick="openSection('wallet')"><i class="fa fa-wallet"></i> Wallet</div>
<div class="sub-icon" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i> Deposit</div>
<div class="sub-icon" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i> Withdraw</div>
<div class="sub-icon" onclick="openSection('profit')"><i class="fa fa-coins"></i> Profit</div>
</div>
<div class="icon-btn" onclick="openSection('support')"><i class="fa fa-headset"></i><span class="icon-name">Support</span></div>
<div class="icon-btn" onclick="openSection('share')"><i class="fa fa-share-alt"></i><span class="icon-name">Referral</span></div>
</div>

<!-- Auth Box -->
<div id="authBox" class="card" style="margin-left:140px;margin-top:20px;width:300px;">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" />
<input id="authEmail" placeholder="Email" />
<input id="authPass" type="password" placeholder="Password" />
<button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
<button onclick="loginUser()" class="btn w-full mb-2 bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>

<div id="welcomeBox" style="display:none;margin-left:140px;" class="card mb-4 p-4 text-center"></div>

<!-- Content Section -->
<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<button onclick="logoutUser()" class="btn bg-red-600" style="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);width:200px;">Logout</button>

<script>
// BG Animation
const canvas=document.getElementById('bgCanvas');
const ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;canvas.height=window.innerHeight;
let particles=[];
for(let i=0;i<150;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+1,s:Math.random()*0.5+0.5,color:`rgba(0,255,255,${Math.random()})`});}
function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle=p.color;ctx.fill();p.y-=p.s;if(p.y<0)p.y=canvas.height;});requestAnimationFrame(animateParticles);}
animateParticles();

// Notifications
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg;n.classList.add('show');setTimeout(()=>{n.classList.remove('show');},3000);}

// Users + Auth
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
window.addEventListener('load', () => { if (currentUser) { showLoginVideo(); } });

function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return showNotif('Fill all fields');if(!validateEmail(e))return showNotif('Invalid email');if(users.find(u=>u.email===e))return showNotif('Email already registered');let u={name:n,email:e,pass:p,plans:[],balance:0,profit:0};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));showLoginVideo();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return showNotif('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));showLoginVideo();}

function showLoginVideo(){
const overlay=document.getElementById('videoOverlay');
overlay.style.display='flex';
const video=document.getElementById('loginVideo');
video.currentTime=0;
video.play();
video.onended=()=>{overlay.style.display='none';openDashboard();}
}

// Dashboard
function openDashboard(){
document.getElementById('authBox').style.display='none';
document.getElementById('sidebar').classList.add('active');
document.getElementById('welcomeBox').style.display='block';
document.getElementById('welcomeBox').innerHTML=`
<h2 style="font-size:28px;font-weight:800;color:#0ff;text-shadow:0 0 5px #0ff,0 0 10px #0ff; animation: neonGlow 1.5s ease-in-out infinite alternate;">Welcome, <span style="color:#fff;">${currentUser.name}</span> 💎</h2>
<div style="display:flex;justify-content:center;gap:20px;margin-top:20px;flex-wrap:wrap;">
<div class="stat-card"><p class="stat-label">Balance</p><p class="stat-value">${currentUser.balance} PKR</p></div>
<div class="stat-card"><p class="stat-label">Profit</p><p class="stat-value">${currentUser.profit} PKR</p></div>
</div>`;}

function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function refreshBalance(){openDashboard();showNotif('Balance refreshed!');}
function toggleSubIcons(){document.getElementById('financeSub').classList.toggle('active');}
function openSection(type){const sec=document.getElementById('contentSection');const content=document.getElementById('sectionContent');sec.style.display='block';
switch(type){case 'home':content.innerHTML='<h2>Dashboard</h2><p>Overview of balance and plans.</p>';break;case 'plans':content.innerHTML='<h2>Plans</h2><p>Plans content here.</p>';break;case 'wallet':content.innerHTML=`<h2>Wallet</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;break;case 'deposit':content.innerHTML=`<h2>Deposit</h2><input type="number" placeholder="Amount"><input type="file"><input type="text" placeholder="Transaction ID"><button class="btn mt-2">Submit Deposit</button>`;break;case 'withdraw':content.innerHTML=`<h2>Withdraw</h2><input type="number" placeholder="Amount"><select><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type="text" placeholder="Account Number"><button class="btn mt-2">Submit Withdraw</button>`;break;case 'profit':content.innerHTML=`<h2>Profit</h2><p>Daily profit tracking and details here.</p>`;break;case 'support':content.innerHTML='<h2>Support</h2><p>Contact support@rockearnpro.com</p>';break;case 'share':content.innerHTML=`<h2>Referral / Share</h2><input type='text' value='https://rockearnpro.com?ref=${currentUser.email}' readonly><button class='btn mt-2' onclick='copyText("https://rockearnpro.com?ref=${currentUser.email}")'>Copy Link</button>`;break;default:content.innerHTML='<h2>Coming Soon...</h2>';break;}}
function closeSection(){document.getElementById('contentSection').style.display='none';}
function copyText(val){navigator.clipboard.writeText(val);showNotif('Copied: '+val);}
</script>
</body>
</html>
