<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;background:linear-gradient(135deg,#1b1b1b,#2b1b17);color:white;overflow-x:hidden;}
button{cursor:pointer;transition:0.3s;}button:hover{opacity:0.85;}
.show{opacity:1!important;}
@keyframes glow{0%{text-shadow:0 0 5px #0ff,0 0 10px #0ff;}50%{text-shadow:0 0 15px #0ff,0 0 25px #0ff;}100%{text-shadow:0 0 5px #0ff,0 0 10px #0ff;}}
#logo{font-size:3rem;font-weight:bold;color:#0ff;text-align:center;animation:glow 2s infinite alternate;margin-top:20px;}
#authBox{width:100%;height:100vh;display:flex;justify-content:center;align-items:center;background:linear-gradient(135deg,#0b0b0b,#1b1b1b);}
.auth-inner{background:#222;padding:40px;border-radius:15px;box-shadow:0 0 20px rgba(0,255,255,0.2);width:320px;}
.auth-inner input{width:100%;padding:10px;margin:10px 0;border:none;border-radius:8px;background:#333;color:white;}
.auth-inner button{width:100%;padding:10px;background:#0ff;color:#000;border:none;border-radius:8px;font-weight:bold;margin-top:5px;}
#notif{position:fixed;top:20px;left:50%;transform:translateX(-50%);background:#0ff;color:#000;padding:10px 20px;border-radius:10px;opacity:0;transition:0.5s;z-index:1000;}
#sidebar{display:none;position:fixed;top:0;left:0;width:60px;height:100vh;background:#111;flex-direction:column;align-items:center;padding:10px;gap:20px;z-index:999;}
#sidebar i{cursor:pointer;}
#sidebar i:hover{color:#0ff;}
#welcomeBox{display:none;position:fixed;top:20px;left:80px;background:#222;padding:20px;border-radius:10px;box-shadow:0 0 20px rgba(0,255,255,255,0.1);}
#contentSection{margin-left:80px;margin-top:120px;display:none;position:relative;}
#profileModal{display:none;position:fixed;top:0;left:0;width:100vw;height:100vh;background:rgba(0,0,0,0.9);justify-content:center;align-items:center;}
#profileModal div{background:#111;padding:30px;border-radius:15px;width:300px;}
.plan-card{background:#222;margin:10px;padding:15px;border-radius:10px;box-shadow:0 0 10px rgba(0,255,255,0.2);}
</style>
</head>
<body>

<div id="notif"></div>

<div id="logo">Rock Earn</div>

<div id="authBox">
  <div class="auth-inner">
    <h2 style="text-align:center;margin-bottom:20px;">Login / Signup</h2>
    <input type="text" id="authName" placeholder="Full Name">
    <input type="email" id="authEmail" placeholder="Email">
    <input type="password" id="authPass" placeholder="Password">
    <button onclick="signupUser()">Sign Up & Login</button>
    <button onclick="loginUser()">Login</button>
  </div>
</div>

<div id="sidebar">
  <i class="fa-solid fa-house fa-2xl text-cyan-400" onclick="openSection('home')"></i>
  <i class="fa-solid fa-money-check-dollar fa-2xl text-cyan-400" onclick="openSection('plans')"></i>
  <i class="fa-solid fa-circle-plus fa-2xl text-cyan-400" onclick="openSection('deposit')"></i>
  <i class="fa-solid fa-hand-holding-dollar fa-2xl text-cyan-400" onclick="openSection('withdraw')"></i>
  <i class="fa-solid fa-chart-line fa-2xl text-cyan-400" onclick="openSection('profit')"></i>
  <i class="fa-solid fa-clock-rotate-left fa-2xl text-cyan-400" onclick="openSection('history')"></i>
  <i class="fa-solid fa-user fa-2xl text-cyan-400" onclick="openProfile()"></i>
  <i class="fa-solid fa-arrow-rotate-left fa-2xl text-cyan-400" id="refreshBtn" onclick="refreshDashboard()" style="display:none;"></i>
  <i class="fa-solid fa-right-from-bracket fa-2xl text-red-500" onclick="logoutUser()"></i>
  <i class="fa-solid fa-user-gear fa-2xl text-yellow-400" id="adminBtn" onclick="openSection('admin')" style="display:none;"></i>
</div>

<div id="welcomeBox">
  Welcome <span id="userNameTxt"></span>
  <br>Balance: <span id="balValue">0 PKR</span> | Profit: <span id="profValue">0 PKR</span>
</div>

<div id="contentSection"></div>

<div id="profileModal">
  <div>
    <h3 style="text-align:center;">Edit Profile</h3>
    <input id="profileName" placeholder="Name"><br>
    <input id="profileEmail" placeholder="Email"><br>
    <input id="profilePass" placeholder="Password"><br>
    <button onclick="updateProfile()">Update</button>
    <button onclick="closeProfile()">Close</button>
  </div>
</div>

<script>
// ---------------------- Users ----------------------
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
    users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
    localStorage.setItem('reUsers',JSON.stringify(users));
}
// Plans
const plans = [
{name:'Basic',amount:200,daily:30,days:20},
{name:'Starter',amount:500,daily:75,days:20},
{name:'Pro',amount:1000,daily:180,days:20},
{name:'Advanced',amount:1500,daily:250,days:25},
{name:'Silver',amount:2000,daily:350,days:30},
{name:'Gold',amount:3000,daily:550,days:30},
{name:'Platinum',amount:5000,daily:950,days:40},
{name:'Diamond',amount:7000,daily:1350,days:50},
{name:'VIP',amount:10000,daily:2000,days:60},
{name:'Elite',amount:12000,daily:2400,days:60},
{name:'Master',amount:15000,daily:3000,days:70},
{name:'Ultimate',amount:18000,daily:3500,days:75},
{name:'Legend',amount:20000,daily:4000,days:80},
{name:'Supreme',amount:25000,daily:5000,days:90},
{name:'Titan',amount:30000,daily:6000,days:100}
];

// ---------------------- Notifications ----------------------
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }

// ---------------------- Auth ----------------------
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

// ---------------------- Dashboard ----------------------
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
    const content=document.getElementById('contentSection');
    content.innerHTML='';
    if(type==='plans'){
        plans.forEach((p,idx)=>{
            content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
        });
    } else if(type==='deposit'){
        openDeposit();
    } else if(type==='withdraw'){
        openWithdraw();
    } else if(type==='profit'){
        content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;
    } else if(type==='history'){
        let html='<h3>History</h3>';
        (currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;});
        (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;});
        content.innerHTML=html;
    } else if(type==='admin' && currentUser.role==='admin'){
        let html='<h3>Admin — User Activity</h3>';
        users.forEach(u=>{
            html+=`<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;
        });
        content.innerHTML=html;
    } else {content.innerHTML='<h3>Coming Soon</h3>';}
}
function closeSection(){document.getElementById('contentSection').style.display='block';}

// ---------------------- Deposit ----------------------
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
    const content=document.getElementById('contentSection');
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

// ---------------------- Withdraw ----------------------
function openWithdraw(){
    const content=document.getElementById('contentSection');
    content.innerHTML=`<h3>Withdraw</h3>
    <input id="withdrawName" placeholder="Your Name">
    <input id="withdrawAcc" placeholder="Account Number">
    <input id="withdrawAmount" placeholder="Amount">
    <select id="withdrawMethod">
        <option value="JazzCash">JazzCash</option>
        <option value="EasyPaisa">EasyPaisa</option>
        <option value="Bank">Bank</option>
        <option value="Binance" disabled>Binance (Coming Soon)</option>
    </select><br><br>
    <button onclick="confirmWithdraw()">Submit Withdraw</button>`;
}
function confirmWithdraw(){
    const name=document.getElementById('withdrawName').value.trim();
    const acc=document.getElementById('withdrawAcc').value.trim();
    const amt=parseInt(document.getElementById('withdrawAmount').value.trim());
    const method=document.getElementById('withdrawMethod').value;
    if(!name||!acc||!amt){showNotif('Fill all fields'); return;}
    if(amt>currentUser.balance){showNotif('Insufficient balance'); return;}
    currentUser.balance -= amt;
    currentUser.withdrawals=currentUser.withdrawals||[];
    currentUser.withdrawals.push({name:name,account:acc,amount:amt,method:method,status:'done'});
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Withdraw processed');
    openSection('withdraw');
}

// ---------------------- Copy Helper ----------------------
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// ---------------------- Profile ----------------------
function openProfile(){
    document.getElementById('profileModal').style.display='flex';
    document.getElementById('profileName').value=currentUser.name;
    document.getElementById('profileEmail').value=currentUser.email;
}
function closeProfile(){document.getElementById('profileModal').style.display='none';}
function updateProfile(){
    const n=document.getElementById('profileName').value.trim();
    const e=document.getElementById('profileEmail').value.trim();
    const p=document.getElementById('profilePass').value.trim();
    if(n) currentUser.name=n;
    if(e) currentUser.email=e;
    if(p) currentUser.pass=p;
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Profile updated');
}

// ---------------------- Init ----------------------
if(currentUser) openDashboard();
</script>

</body>
</html>
