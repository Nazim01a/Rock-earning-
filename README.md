<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;color:white;overflow-x:hidden;background:#111;}
#bgCanvas{position:fixed;top:0;left:0;z-index:-1;width:100%;height:100%;}
#authBox,#welcomeBox,#contentSection,#sidebar,#profileModal{display:none;}
#authBox{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;}
input,button,select{margin:5px;padding:10px;border-radius:5px;border:none;}
button{cursor:pointer;background:#2196F3;color:white;}
#sidebar{position:fixed;left:0;top:0;width:80px;height:100vh;background:#222;flex-direction:column;align-items:center;padding:10px;}
.sidebar-icon{margin:15px;color:white;font-size:24px;cursor:pointer;}
#contentSection{margin-left:90px;padding:20px;}
.plan-card{background:#333;padding:15px;margin:10px;border-radius:10px;}
#notif{position:fixed;top:10px;right:10px;background:#4caf50;padding:10px;border-radius:5px;display:none;}
#notif.show{display:block;animation:fade 0.5s;}
@keyframes fade{0%{opacity:0}100%{opacity:1}}
#rockEarnName{font-size:36px;font-weight:bold;text-align:center;margin-bottom:10px;}
#profileModal{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#222;padding:20px;border-radius:10px;}
</style>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
</head>
<body>
<canvas id="bgCanvas"></canvas>

<div id="authBox">
<h2>Login / Sign Up</h2>
<input id="authName" placeholder="Name (Sign Up)">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>

<div id="welcomeBox">
<div id="rockEarnName">Welcome to Rock Earn</div>
<div>Welcome, <span id="userNameTxt"></span></div>
<div>Balance: <span id="balValue">0</span> PKR | Profit: <span id="profValue">0</span> PKR</div>
<button id="refreshBtn" onclick="refreshDashboard()">Refresh</button>
<button onclick="logoutUser()">Logout</button>
</div>

<div id="sidebar">
<i class="fa-solid fa-house sidebar-icon" onclick="openSection('home')"></i>
<i class="fa-solid fa-layer-group sidebar-icon" onclick="openSection('plans')"></i>
<i class="fa-solid fa-arrow-down-sidebar sidebar-icon" onclick="openSection('deposit')"></i>
<i class="fa-solid fa-arrow-up-sidebar sidebar-icon" onclick="openSection('withdraw')"></i>
<i class="fa-solid fa-dollar-sign sidebar-icon" onclick="openSection('profit')"></i>
<i class="fa-solid fa-history sidebar-icon" onclick="openSection('history')"></i>
<i class="fa-solid fa-user sidebar-icon" onclick="openProfile()"></i>
<i class="fa-solid fa-user-gear sidebar-icon" id="adminBtn" onclick="openSection('admin')"></i>
</div>

<div id="contentSection">
<div id="sectionContent"></div>
</div>

<div id="profileModal">
<h3>Edit Profile</h3>
<input id="profileName" placeholder="Name">
<input id="profileEmail" placeholder="Email">
<input id="profilePass" type="password" placeholder="New Password">
<button onclick="updateProfile()">Save</button>
<button onclick="closeProfile()">Close</button>
</div>

<div id="notif"></div>

<script>
// ----------------------
// Users & Storage
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
    users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
    localStorage.setItem('reUsers',JSON.stringify(users));
}
// Plans
const plans=[
{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},{name:'Advanced',amount:1500,daily:250,days:25},{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30},{name:'Platinum',amount:5000,daily:950,days:40},{name:'Diamond',amount:7000,daily:1350,days:50},{name:'VIP',amount:10000,daily:2000,days:60},{name:'Elite',amount:12000,daily:2400,days:60},{name:'Master',amount:15000,daily:3000,days:70},{name:'Ultimate',amount:18000,daily:3500,days:75},{name:'Legend',amount:20000,daily:4000,days:80},{name:'Supreme',amount:25000,daily:5000,days:90},{name:'Titan',amount:30000,daily:6000,days:100}
];

// ----------------------
// Notifications
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }

// ----------------------
// Auth Functions
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

function logoutUser(){currentUser=null; localStorage.removeItem('reCurrent'); location.reload();}

// ----------------------
// Dashboard & Sections
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

function refreshDashboard(){
if(currentUser){
currentUser=users.find(u=>u.email===currentUser.email);
localStorage.setItem('reCurrent',JSON.stringify(currentUser));
document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
showNotif('Dashboard refreshed');
closeSection();
}
}

function openSection(type){
const content=document.getElementById('sectionContent');
content.innerHTML='';
if(type==='plans'){
plans.forEach((p,idx)=>{
content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
});
} else if(type==='deposit'){content.innerHTML='<h3>Deposit Section</h3>';
} else if(type==='withdraw'){openWithdraw();
} else if(type==='profit'){content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;
} else if(type==='history'){let html='<h3>History</h3>';
(currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;});
(currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;});
content.innerHTML=html;
} else if(type==='admin' && currentUser.role==='admin'){let html='<h3>Admin — User Activity</h3>';
users.forEach(u=>{html+=`<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;});
content.innerHTML=html;
} else {content.innerHTML='<h3>Coming Soon</h3>';}
}
function closeSection(){document.getElementById('contentSection').style.display='block';}

// ----------------------
// Deposit / Buy Plan
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
const content=document.getElementById('sectionContent');
content.innerHTML=`<h3>Deposit — ${name}</h3>
<p>Amount: <strong>${amount} PKR</strong></p>
<p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
<p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
<p>Bank (Coming Soon)</p>
<p>Binance (Coming Soon)</p>
<div style="margin-top:10px">
<input id="depositTxn" placeholder="Transaction ID">
<input id="depositProof" type="file"><br>
<button onclick="confirmDeposit(${amount},'${name}',${daily},${days},${planIndex})">Submit Deposit</button>
</div>`;
}

function confirmDeposit(amount,name,daily,days,planIndex){
const txn=document.getElementById('depositTxn').value.trim();
const proof=document.getElementById('depositProof').files[0];
if(!txn || !proof){showNotif('Upload proof & enter txn'); return;}
const deposit={plan:name,amount:amount,daily:daily,days:days,txn:txn,proof:proof.name,status:'approved',ts:Date.now()};
currentUser.deposits=currentUser.deposits||[]; currentUser.deposits.push(deposit);
currentUser.balance += amount;
currentUser.profit += daily;
users=users.map(u=>u.email===currentUser.email?currentUser:u);
localStorage.setItem('reUsers',JSON.stringify(users));
localStorage.setItem('reCurrent',JSON.stringify(currentUser));
showNotif('Deposit confirmed — balance updated');
openSection('plans');
}

// ----------------------
// Withdraw Section & Profile functions included from Part 3
// (Use same JS from Part 3 for withdraw, profile, background animation, Rock Earn animation)

// Init on load
if(currentUser) openDashboard();
</script>
<script>
// Background & Rock Earn animation (Part 3)
const canvas=document.getElementById('bgCanvas');
const ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;
canvas.height=window.innerHeight;
let particles=[];
for(let i=0;i<100;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*3+1,d:Math.random()*2});}
function draw(){ctx.fillStyle="rgba(20,15,10,0.9)";ctx.fillRect(0,0,canvas.width,canvas.height);ctx.fillStyle="rgba(255,255,255,0.8)";particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,Math.PI*2,true);ctx.fill();p.y+=p.d;if(p.y>canvas.height){p.y=0;p.x=Math.random()*canvas.width;}});requestAnimationFrame(draw);}
draw();
const rockName=document.getElementById('rockEarnName');let hue=0;function animateRockName(){hue=(hue+1)%360;rockName.style.color=`hsl(${hue},80%,60%)`;requestAnimationFrame(animateRockName);}animateRockName();
</script>
</body>
</html>
