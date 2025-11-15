!ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
/* ---------- STYLES ---------- */
body{margin:0;font-family:'Segoe UI',sans-serif;background:#0e0e15;color:white;overflow-x:hidden;}
canvas#bgCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}
.sidebar{position:fixed;top:0;left:0;width:80px;height:100vh;background:rgba(20,20,20,0.95);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;display:none;}
.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:#111;color:#ccc;box-shadow:0 0 5px #000;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 15px #0ff,0 0 25px #0ff;}
.icon-name{margin-top:6px;font-size:12px;color:#ccc;text-shadow:0 0 3px #000;text-align:center;}
#welcomeBox{position:fixed;top:20px;left:100px;right:20px;padding:25px;border-radius:20px;background:rgba(0,0,0,0.8);backdrop-filter:blur(12px);text-align:center;display:none;animation:fadeIn 1.5s;}
#welcomeBox h2{color:white;font-size:26px;font-weight:800;animation:glowText 2.5s infinite alternate;}
.stats{display:flex;justify-content:center;gap:30px;margin-top:15px;flex-wrap:wrap;}
.stat-card{background: rgba(0,255,255,0.05); padding: 18px 25px; border-radius: 15px; box-shadow: 0 0 5px #0ff,0 0 10px #0ff; transition: 0.4s;}
.stat-label{font-size:12px;color:#0ff;letter-spacing:1px;margin-bottom:5px;}
.stat-value{font-size:18px;font-weight:700;color:white;}
.refresh-btn{margin-top:10px;}
#contentSection{position:fixed;top:160px;left:100px;right:20px;bottom:20px;background:rgba(0,0,0,0.85);backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#0ff,#0ffcc0);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.04);}
.btn{background:#111;color:#0ff;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 5px #0ff,0 0 10px #0ff;transition:0.3s;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
.logout-btn{position:fixed;bottom:80px;left:50%;transform:translateX(-50%);background:#ff0044;color:white;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 10px #ff3366;transition:0.3s;display:none;}
.logout-btn:hover{transform:scale(1.05);box-shadow:0 0 20px #ff3366,0 0 30px #ff6688;}
.admin-btn{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#00ff66;color:#000;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 10px #00ff66;display:none;}
@keyframes glowText{0%{text-shadow:0 0 5px #000,0 0 10px #0ff;}50%{text-shadow:0 0 12px #000,0 0 25px #0ff;}100%{text-shadow:0 0 5px #000,0 0 10px #0ff;}}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@media (max-width: 1024px){#welcomeBox{left:20px;right:20px;}#contentSection{top:220px;left:20px;right:20px;bottom:20px;} .sidebar{width:100%;height:auto;flex-direction:row;justify-content:space-around;padding:10px;display:flex;position:relative;}}
/* extra small tweaks */
.auth-card{position:fixed;left:100px;margin-top:20px;width:340px;background:rgba(0,0,0,0.6);padding:18px;border-radius:14px;box-shadow:0 0 20px rgba(0,0,0,0.6);}
@media (max-width: 640px){.auth-card{left:20px;width:calc(100% - 40px);}}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>

<!-- Notification -->
<div id="notif"></div>

<!-- Sidebar -->
<div class="sidebar" id="sidebar">
  <div style="font-size:28px;margin-bottom:10px;">🚀</div>
  <div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><span class="icon-name">Plans</span></div>
  <div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><span class="icon-name">Deposit</span></div>
  <div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><span class="icon-name">Withdraw</span></div>
  <div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><span class="icon-name">Profit</span></div>
  <div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><span class="icon-name">History</span></div>
  <div class="icon-btn" onclick="openSection('support')"><i class="fa fa-headset"></i><span class="icon-name">Support</span></div>
  <div class="icon-btn" onclick="openSection('referral')"><i class="fa fa-share-alt"></i><span class="icon-name">Referral</span></div>
  <div class="icon-btn" onclick="openSection('share')"><i class="fa fa-share"></i><span class="icon-name">Share</span></div>
</div>

<!-- Welcome Box -->
<div id="welcomeBox">
  <h2>🎉 Welcome to <span style="color:white;font-weight:900;">Rock Earn</span> Premium 💎</h2>
  <div class="stats">
    <div class="stat-card">
      <p class="stat-label">Balance</p>
      <p class="stat-value" id="balValue">0 PKR</p>
    </div>
    <div class="stat-card">
      <p class="stat-label">Total Profit</p>
      <p class="stat-value" id="profValue">0 PKR</p>
    </div>
    <button class="refresh-btn btn" onclick="refreshBalance()">Refresh</button>
  </div>
</div>

<!-- Main Content Section (modal style) -->
<div id="contentSection">
  <button id="backBtn" onclick="closeSection()">← Back</button>
  <div id="sectionContent"></div>
</div>

<!-- Auth box -->
<div id="authBox" class="auth-card">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" />
  <input id="authEmail" placeholder="Email" />
  <input id="authPass" type="password" placeholder="Password" />
  <button onclick="signupUser()" class="btn w-full mb-2" style="width:100%;"><i class="fa fa-user-plus"></i> Sign Up</button>
  <button onclick="loginUser()" class="btn w-full mb-2" style="width:100%;background:#10b981;border-radius:12px;"><i class="fa fa-sign-in-alt"></i> Login</button>
  <p style="font-size:12px;color:#aaa;margin-top:6px;text-align:center;">Use <b>admin@rockearn.com</b> to access Admin Panel (password: <i>admin123</i>)</p>
</div>

<!-- Logout / Admin buttons -->
<button class="logout-btn" id="logoutBtn" onclick="logoutUser()">Logout</button>
<button class="admin-btn" id="adminBtn" onclick="openSection('admin')">Admin Panel</button>

<script>
/* ========== Particle Background ========== */
const canvas=document.getElementById('bgCanvas'),ctx=canvas.getContext('2d');
function resizeCanvas(){canvas.width=window.innerWidth;canvas.height=window.innerHeight;}
resizeCanvas();window.addEventListener('resize',resizeCanvas);
let particles=[];for(let i=0;i<180;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+0.7,s:Math.random()*0.6+0.2,color:`rgba(0,255,255,${0.1+Math.random()*0.8})`});}
function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle=p.color;ctx.fill();p.y-=p.s; p.x += Math.sin(Date.now()/10000 + p.y/100)*0.3; if(p.y< -10) {p.y = canvas.height + 10; p.x = Math.random() * canvas.width;} if(p.x< -20) p.x = canvas.width+20; if(p.x>canvas.width+20) p.x=-20});requestAnimationFrame(animateParticles);}animateParticles();

/* ========== Notifications ========== */
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg; n.classList.add('show'); setTimeout(()=>{n.classList.remove('show');},3000);}

/* ========== USERS / AUTH ========== */
/* Stored in localStorage:
   reUsers: array of users
   reCurrent: current user object
   reRequests: deposit/withdraw requests (admin)
*/
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
// create admin user if not exists
if(!users.find(u=>u.email==='admin@rockearn.com')){
  users.push({name:'Admin', email:'admin@rockearn.com', pass:'admin123', plans:[], balance:0, profit:0, history:[]});
  localStorage.setItem('reUsers', JSON.stringify(users));
}
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

// show dashboard if logged in
window.addEventListener('load', ()=>{
  if(currentUser){
    openDashboard();
  } else {
    // small UI tweak: show sidebar collapsed on small screens
    document.getElementById('sidebar').style.display='none';
  }
});

// email validator
function validateEmail(e){ return /^\S+@\S+\.\S+$/.test(e); }

function signupUser(){
  let n = document.getElementById('authName').value.trim();
  let e = document.getElementById('authEmail').value.trim();
  let p = document.getElementById('authPass').value.trim();
  if(!n || !e || !p) return showNotif('Fill all fields');
  if(!validateEmail(e)) return showNotif('Invalid email');
  if(users.find(u=>u.email===e)) return showNotif('Email already registered');
  let u = { name:n, email:e, pass:p, plans:[], balance:0, profit:0, history:[] };
  users.push(u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  currentUser = u;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
  showNotif('Signup successful!');
}

function loginUser(){
  let e = document.getElementById('authEmail').value.trim();
  let p = document.getElementById('authPass').value.trim();
  let u = users.find(x=>x.email===e && x.pass===p);
  if(!u) return showNotif('Invalid credentials');
  currentUser = u;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
  showNotif('Welcome back, ' + currentUser.name.split(' ')[0] + '!');
}

// logout without losing localStorage users
function logoutUser(){
  currentUser = null;
  localStorage.removeItem('reCurrent');
  // safe page update
  location.reload();
}

/* ========== DASHBOARD UI ========== */
function openDashboard(){
  // hide auth box
  document.getElementById('authBox').style.display='none';
  // show sidebar and welcome
  document.getElementById('sidebar').style.display='flex';
  document.getElementById('welcomeBox').style.display='block';
  document.getElementById('logoutBtn').style.display='block';
  // fill stats
  document.getElementById('balValue').innerText = (currentUser.balance || 0) + ' PKR';
  document.getElementById('profValue').innerText = (currentUser.profit || 0) + ' PKR';
  // show admin button only if admin
  if(currentUser.email === 'admin@rockearn.com'){
    document.getElementById('adminBtn').style.display='block';
  } else {
    document.getElementById('adminBtn').style.display='none';
  }
}

/* refresh display values */
function refreshBalance(){
  if(!currentUser) return;
  document.getElementById('balValue').innerText = (currentUser.balance || 0) + ' PKR';
  document.getElementById('profValue').innerText = (currentUser.profit || 0) + ' PKR';
  showNotif('Balance refreshed!');
}

/* ========== PLANS (static list) ========== */
let plans = [
  {name:'Basic', amount:200, daily:30, days:1},
  {name:'Starter', amount:500, daily:75, days:1},
  {name:'Pro', amount:1000, daily:180, days:1},
  {name:'Advanced', amount:1500, daily:250, days:2},
  {name:'Silver', amount:2000, daily:350, days:3},
  {name:'Gold', amount:3000, daily:550, days:3},
  {name:'Platinum', amount:5000, daily:950, days:4},
  {name:'Diamond', amount:7000, daily:1350, days:5}
];

/* -------------------------
   PART-2: Sections + Actions
   (paste at end of main script - integrated)
   ------------------------- */

/* Open content modal with chosen section */
function openSection(section){
  if(!currentUser) return showNotif('Please login first');
  document.getElementById('contentSection').style.display='block';
  let box=document.getElementById('sectionContent');
  box.innerHTML = ''; // clear

  if(section==='plans'){
    box.innerHTML=`<h2 class="text-xl font-bold mb-3">Available Plans</h2>`;
    plans.forEach(p=>{
      box.innerHTML+=`
      <div class="plan-card">
        <p class="font-bold text-lg">${p.name}</p>
        <p>Amount: <b>${p.amount} PKR</b></p>
        <p>Daily Profit: <b>${p.daily} PKR</b></p>
        <p>Days: <b>${p.days}</b></p>
        <button class="btn mt-3" onclick="buyPlan('${p.name}')">Buy Now</button>
      </div>`;
    });
    return;
  }

  if(section==='deposit'){
    box.innerHTML=`
      <h2 class="text-xl font-bold mb-3">Deposit</h2>
      <p>JazzCash / EasyPaisa (example)</p>
      <input id="depAmount" placeholder="Amount" type="number">
      <input id="depTxId" placeholder="Transaction ID">
      <button class="btn mt-2" onclick="submitDeposit()">Submit</button>`;
    return;
  }

  if(section==='withdraw'){
    box.innerHTML=`
      <h2 class="text-xl font-bold mb-3">Withdraw</h2>
      <input id="wName" placeholder="Full Name">
      <input id="wAcc" placeholder="Account Number / Mobile">
      <input id="wAmount" type="number" placeholder="Amount">
      <button class="btn mt-2" onclick="submitWithdraw()">Request Withdraw</button>`;
    return;
  }

  if(section==='profit'){
    box.innerHTML=`
      <h2 class="text-xl font-bold mb-3">Daily Profit</h2>
      <p>Your total daily profit: <b>${currentUser.profit || 0} PKR</b></p>
      <p style="font-size:13px;color:#bbb;">(Daily profit is added when admin approves plans or manually by system)</p>
    `;
    return;
  }

  if(section==='history'){
    let h = currentUser.history || [];
    box.innerHTML = `<h2 class="text-xl font-bold mb-3">History</h2>`;
    if(h.length === 0) box.innerHTML += `<p>No history found</p>`;
    h.slice().reverse().forEach(x=>{
      box.innerHTML += `<div class="plan-card">${x}</div>`;
    });
    return;
  }

  if(section==='referral'){
    box.innerHTML=`
      <h2 class="text-xl font-bold mb-3">Referral</h2>
      <p>Invite Code (use your email):</p>
      <div class="plan-card">${currentUser.email}</div>
      <p style="font-size:13px;color:#bbb;">Share your invite code with friends to get referral benefits (manual system).</p>
    `;
    return;
  }

  if(section==='share'){
    box.innerHTML=`
      <h2 class="text-xl font-bold mb-3">Share App</h2>
      <button class="btn" onclick="shareNow()">Share Now</button>
      <p style="font-size:13px;color:#bbb;margin-top:8px;">Use your browser/mobile share to invite others.</p>
    `;
    return;
  }

  if(section==='support'){
    box.innerHTML=`
      <h2 class="text-xl font-bold mb-3">Support</h2>
      <p>Contact: <b>support@rockearn.com</b></p>
      <p style="font-size:13px;color:#bbb;">(This is demo — replace with real contact)</p>
    `;
    return;
  }

  if(section==='admin'){
    loadAdmin();
    return;
  }
}

function closeSection(){
  document.getElementById('contentSection').style.display='none';
}

/* Buy plan */
function buyPlan(planName){
  let p = plans.find(x=>x.name===planName);
  if(!p) return showNotif('Plan not found');
  if(currentUser.balance < p.amount) return showNotif('Insufficient balance');

  currentUser.balance = Number(currentUser.balance) - Number(p.amount);
  currentUser.profit = Number(currentUser.profit) + Number(p.daily);
  currentUser.plans = currentUser.plans || [];
  // store plan with expiry date (simple)
  let expiry = new Date();
  expiry.setDate(expiry.getDate() + (p.days || 1));
  currentUser.plans.push({name:p.name, amount:p.amount, daily:p.daily, days:p.days, boughtAt: new Date().toISOString(), expireAt: expiry.toISOString()});

  currentUser.history = currentUser.history || [];
  currentUser.history.push(`Bought ${p.name} for ${p.amount} PKR on ${new Date().toLocaleString()}. Daily: ${p.daily} PKR`);

  saveUser();
  refreshBalance();
  showNotif('Plan purchased successfully!');
}

/* Deposit */
function submitDeposit(){
  let amt = document.getElementById('depAmount').value;
  let tx = document.getElementById('depTxId').value.trim();
  if(!amt || !tx) return showNotif('Fill all fields');
  if(Number(amt) <= 0) return showNotif('Enter valid amount');

  let req = { type:'deposit', user: currentUser.email, amount: Number(amt), txid: tx, status:'pending', createdAt: new Date().toISOString() };
  let allReq = JSON.parse(localStorage.getItem('reRequests')) || [];
  allReq.push(req);
  localStorage.setItem('reRequests', JSON.stringify(allReq));

  showNotif('Deposit request sent for admin approval');
  // optional: add to user history
  currentUser.history = currentUser.history || [];
  currentUser.history.push(`Deposit request: ${amt} PKR (Tx: ${tx}) - pending`);
  saveUser();
  closeSection();
}

/* Withdraw */
function submitWithdraw(){
  let n = document.getElementById('wName').value.trim();
  let a = document.getElementById('wAcc').value.trim();
  let amt = document.getElementById('wAmount').value;
  if(!n || !a || !amt) return showNotif('Fill all fields');
  if(Number(amt) <= 0) return showNotif('Enter valid amount');
  if(Number(amt) > Number(currentUser.balance)) return showNotif('Insufficient balance');

  let req = { type:'withdraw', user: currentUser.email, name: n, acc: a, amount: Number(amt), status:'pending', createdAt: new Date().toISOString() };
  let allReq = JSON.parse(localStorage.getItem('reRequests')) || [];
  allReq.push(req);
  localStorage.setItem('reRequests', JSON.stringify(allReq));

  showNotif('Withdraw request sent for admin');
  currentUser.history = currentUser.history || [];
  currentUser.history.push(`Withdraw request: ${amt} PKR - pending`);
  saveUser();
  closeSection();
}

/* Share using Web Share API if available */
function shareNow(){
  if(navigator.share){
    navigator.share({ title:'Rock Earn', text:'Join Rock Earn and start earning!', url: window.location.href })
    .then(()=> showNotif('Thanks for sharing!'))
    .catch(()=> showNotif('Share canceled'));
  } else {
    // fallback: copy link
    navigator.clipboard.writeText(window.location.href).then(()=> showNotif('Link copied to clipboard'));
  }
}

/* Save current user in users array and localStorage */
function saveUser(){
  users = JSON.parse(localStorage.getItem('reUsers')) || users;
  // update user entry
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
}

/* ========== ADMIN PANEL ========== */
function loadAdmin(){
  if(!currentUser || currentUser.email !== 'admin@rockearn.com') return showNotif('Admin only');
  let box = document.getElementById('sectionContent');
  let req = JSON.parse(localStorage.getItem('reRequests')) || [];
  box.innerHTML = `<h2 class="text-xl font-bold mb-3">Admin Panel</h2>`;
  box.innerHTML += `<p style="color:#ccc;font-size:13px;">Total requests: ${req.length}</p>`;

  if(req.length === 0){
    box.innerHTML += `<p>No requests</p>`;
    return;
  }

  req.forEach((r,i)=>{
    box.innerHTML += `
      <div class="plan-card">
        <p><b>Type:</b> ${r.type}</p>
        <p><b>User:</b> ${r.user}</p>
        <p><b>Amount:</b> ${r.amount || ''}</p>
        <p><b>TxID:</b> ${r.txid || ''}</p>
        <p><b>Created:</b> ${new Date(r.createdAt).toLocaleString()}</p>
        <div style="display:flex;gap:8px;margin-top:8px;">
          <button class="btn" onclick="approveReq(${i}
