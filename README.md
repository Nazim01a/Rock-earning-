<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Premium Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body { margin:0; font-family:'Segoe UI',sans-serif; background:#081423; color:#fff; overflow-x:hidden; }
button { cursor:pointer; padding:8px 12px; border:none; border-radius:8px; }
#notif { position:fixed; top:20px; right:-400px; background:#00f7ef; color:#001; padding:12px 20px; border-radius:12px; font-weight:700; transition: right .45s; z-index:60; }
#notif.show { right:20px; }
#authBox { position:fixed; left:50%; top:150px; transform:translateX(-50%); width:320px; background:rgba(0,0,0,0.6); padding:20px; border-radius:14px; }
#authBox input { width:100%; margin-bottom:10px; padding:10px; border-radius:10px; border:none; background:#0f1320; color:#dff7fb; }
#sidebar { position:fixed; left:0; top:0; bottom:0; width:80px; background:rgba(10,10,12,0.9); display:none; flex-direction:column; align-items:center; padding:20px 0; gap:14px; z-index:50; }
.icon-btn { width:64px; height:64px; border-radius:14px; background:linear-gradient(180deg,#0b0b10,#0f1220); display:flex; flex-direction:column; align-items:center; justify-content:center; color:#bcdbe3; font-size:14px; transition:.2s; text-align:center; }
.icon-btn:hover { transform:translateY(-6px); box-shadow:0 12px 30px rgba(0,247,239,0.2); }
#sidebar i { font-size:20px; }
#contentSection { position:fixed; left:100px; top:120px; right:20px; bottom:20px; background:rgba(0,0,0,0.5); border-radius:14px; padding:20px; overflow:auto; display:none; }
#backBtn { position:absolute; left:12px; top:12px; background:#ff4766; color:white; padding:8px 10px; border-radius:8px; cursor:pointer; display:none; }
#logoutBtn { position:fixed; bottom:20px; left:50%; transform:translateX(-50%); background:#ff4766; color:white; padding:10px 14px; border-radius:10px; cursor:pointer; display:none; }
#refreshBtn { position:fixed; bottom:20px; right:20px; background:#00f7ef; color:#001; padding:10px 14px; border-radius:10px; cursor:pointer; display:none; }
#welcomeBox { position:fixed; left:100px; top:20px; right:20px; padding:18px; border-radius:14px; background:rgba(255,255,255,0.02); backdrop-filter:blur(8px); box-shadow:0 4px 30px rgba(0,0,0,0.5); display:none; }
.stats { display:flex; gap:18px; margin-top:12px; }
.stat-card { background:rgba(0,247,239,0.03); padding:12px 16px; border-radius:10px; min-width:120px; }
.stat-label { font-size:11px; color:#00f7ef; margin:0 0 6px; }
.stat-value { font-size:16px; font-weight:700; margin:0; }
.plan-card { border:1px solid rgba(0,247,239,0.1); border-radius:10px; padding:12px; margin-bottom:12px; background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0)); }
.muted { color:#9fcfda; font-size:13px; }
input[type="file"], input, select{color:#fff; margin-bottom:6px; border-radius:6px; border:none; padding:6px; background:#0f1320; width:100%;}
table{width:100%; border-collapse:collapse;}
th,td{border:1px solid rgba(0,247,239,0.2); padding:6px 8px; text-align:left;}
th{background:rgba(0,247,239,0.05);}
</style>
</head>
<body>

<div id="notif"></div>

<div id="authBox">
<h3 style="text-align:center">Login / Sign Up</h3>
<input id="authName" placeholder="Full Name">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<div style="display:flex; gap:8px;">
<button onclick="signupUser()"><i class="fa fa-user-plus"></i> Sign Up</button>
<button style="background:#00b37e;color:#001;" onclick="loginUser()"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>
<p class="muted" style="margin-top:10px; font-size:13px;">Default admin: <strong>admin@rockearn.com</strong></p>
</div>

<div id="sidebar">
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><br>Plans</div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><br>Deposit</div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><br>Withdraw</div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><br>Profit</div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><br>History</div>
<div class="icon-btn" onclick="openSection('admin')" id="adminBtn" style="display:none"><i class="fa fa-cog"></i><br>Admin</div>
</div>

<div id="welcomeBox">
<h2 id="welcomeTxt">🎉 Welcome, <strong id="userName"></strong> 💎</h2>
<div class="stats">
<div class="stat-card"><div class="stat-label">Balance</div><div class="stat-value" id="balValue">0 PKR</div></div>
<div class="stat-card"><div class="stat-label">Total Profit</div><div class="stat-value" id="profValue">0 PKR</div></div>
</div>
</div>

<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<button id="logoutBtn" onclick="logoutUser()">Logout</button>
<button id="refreshBtn" onclick="refreshDashboard()"><i class="fa fa-sync"></i> Refresh</button>

<script>
// =====================
// Users & Storage
// =====================
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

if(!users.find(u=>u.email==='admin@rockearn.com')){
users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
localStorage.setItem('reUsers',JSON.stringify(users));
}

const plans = [
{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},
{name:'Advanced',amount:1500,daily:250,days:25},{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30},
{name:'Platinum',amount:5000,daily:950,days:40},{name:'Diamond',amount:7000,daily:1350,days:50},{name:'VIP',amount:10000,daily:2000,days:60},
{name:'Elite',amount:12000,daily:2400,days:60},{name:'Master',amount:15000,daily:3000,days:70},{name:'Ultimate',amount:18000,daily:3500,days:75},
{name:'Legend',amount:20000,daily:4000,days:80},{name:'Supreme',amount:25000,daily:5000,days:90},{name:'Titan',amount:30000,daily:6000,days:100}
];

// =====================
// Notifications
// =====================
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }

// =====================
// Auth
// =====================
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

// =====================
// Dashboard
// =====================
function openDashboard(){
document.getElementById('authBox').style.display='none';
document.getElementById('sidebar').style.display='flex';
document.getElementById('welcomeBox').style.display='block';
document.getElementById('logoutBtn').style.display='block';
document.getElementById('refreshBtn').style.display='block';
document.getElementById('userName').innerText=currentUser.name;
document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';} else {document.getElementById('adminBtn').style.display='none';}
document.getElementById('contentSection').style.display='none';
document.getElementById('backBtn').style.display='none';
}// =====================
// Sections
// =====================
function openSection(type){
const content=document.getElementById('sectionContent');
document.getElementById('contentSection').style.display='block';
document.getElementById('backBtn').style.display='inline-block';
content.innerHTML='';

if(type==='plans'){
plans.forEach((p,idx)=>{
content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
});
} else if(type==='deposit'){
content.innerHTML='<h3>Deposit Section</h3>';
(currentUser.deposits||[]).forEach((d,i)=>{
content.innerHTML+=`<p>${d.plan} - ${d.amount} PKR - ${d.status}</p>`;
});
} else if(type==='withdraw'){
content.innerHTML='<h3>Withdraw Section</h3>';
(currentUser.withdrawals||[]).forEach((w,i)=>{
content.innerHTML+=`<p>${w.method} - ${w.amount} PKR - ${w.status}</p>`;
});
content.innerHTML+=`
<input id="withdrawAmount" type="number" placeholder="Amount">
<select id="withdrawMethod">
<option value="JazzCash">JazzCash</option>
<option value="EasyPaisa">EasyPaisa</option>
<option value="Bank">Bank</option>
</select>
<input id="withdrawAcc" placeholder="Account / Number">
<button onclick="submitWithdraw()">Submit Withdraw</button>`;
} else if(type==='profit'){
content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;
} else if(type==='history'){
let html='<h3>History</h3>';
(currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR - ${d.status}</p>`});
(currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.method} - ${w.amount} PKR - ${w.status}</p>`});
content.innerHTML=html;
} else if(type==='admin' && currentUser.role==='admin'){
let html='<h3>Admin Panel</h3>';
html+=`<button onclick="refreshDashboard()">Refresh</button>`;
users.forEach((u,i)=>{
html+=`<hr><b>${u.name} (${u.email})</b><br>Balance: ${u.balance} PKR<br>`;
(u.deposits||[]).forEach((d,j)=>{
if(d.status==='pending'){
html+=`${d.plan} - ${d.amount} PKR - ${d.status} 
<button onclick="approveDeposit(${i},${j})">Approve</button>
<button onclick="rejectDeposit(${i},${j})">Reject</button><br>`;
}else{
html+=`${d.plan} - ${d.amount} PKR - ${d.status}<br>`;
}
});
(u.withdrawals||[]).forEach((w,k)=>{
if(w.status==='pending'){
html+=`Withdraw: ${w.amount} PKR (${w.method}) - ${w.status} 
<button onclick="approveWithdraw(${i},${k})">Approve</button>
<button onclick="rejectWithdraw(${i},${k})">Reject</button><br>`;
}else{
html+=`Withdraw: ${w.amount} PKR (${w.method}) - ${w.status}<br>`;
}
});
});
content.innerHTML=html;
} else {content.innerHTML='<h3>Coming Soon</h3>';}
}

// =====================
// Deposit / Buy Plan
// =====================
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
const content=document.getElementById('sectionContent');
document.getElementById('contentSection').style.display='block';
content.innerHTML=`<h3>Deposit — ${name}</h3>
<p>Amount: <strong>${amount} PKR</strong></p>
<p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
<p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
<div style="margin-top:10px">
<input id="depositTxn" placeholder="Transaction ID">
<input id="depositProof" type="file"><br>
<button onclick="confirmDeposit(${amount},'${name}',${daily},${days},${planIndex})">Submit Deposit (Pending Admin)</button>
</div>`;
}

function confirmDeposit(amount,name,daily,days,planIndex){
const txn=document.getElementById('depositTxn').value.trim();
const proof=document.getElementById('depositProof').files[0];
if(!txn || !proof){showNotif('Upload proof & enter txn'); return;}
const deposit={plan:name,amount:amount,daily:daily,days:days,txn:txn,proof:proof.name,status:'pending',ts:Date.now()};
currentUser.deposits=currentUser.deposits||[]; currentUser.deposits.push(deposit);
users=users.map(u=>u.email===currentUser.email?currentUser:u);
localStorage.setItem('reUsers',JSON.stringify(users));
localStorage.setItem('reCurrent',JSON.stringify(currentUser));
showNotif('Deposit submitted — waiting admin approval');
openSection('plans');
}

// =====================
// Withdraw
// =====================
function submitWithdraw(){
let amt=parseInt(document.getElementById('withdrawAmount').value);
let method=document.getElementById('withdrawMethod').value;
let acc=document.getElementById('withdrawAcc').value.trim();
if(!amt || !method || !acc){showNotif('Fill all withdrawal fields'); return;}
if(amt>currentUser.balance){showNotif('Insufficient balance'); return;}
const w={amount:amt,method:method,account:acc,status:'pending',ts:Date.now()};
currentUser.withdrawals=currentUser.withdrawals||[]; currentUser.withdrawals.push(w);
users=users.map(u=>u.email===currentUser.email?currentUser:u);
localStorage.setItem('reUsers',JSON.stringify(users));
localStorage.setItem('reCurrent',JSON.stringify(currentUser));
showNotif('Withdraw submitted — waiting admin approval');
openSection('withdraw');
}

// =====================
// Admin Approve / Reject
// =====================
function approveDeposit(userIdx,depIdx){
let d=users[userIdx].deposits[depIdx];
if(d.status!=='pending') return;
d.status='approved';
users[userIdx].balance+=d.amount;
users[userIdx].profit+=d.daily*d.days;
localStorage.setItem('reUsers',JSON.stringify(users));
if(users[userIdx].email===currentUser.email){currentUser=users[userIdx]; localStorage.setItem('reCurrent',JSON.stringify(currentUser));}
openSection('admin');
showNotif('Deposit approved');
}

function rejectDeposit(userIdx,depIdx){
users[userIdx].deposits[depIdx].status='rejected';
localStorage.setItem('reUsers',JSON.stringify(users));
openSection('admin');
showNotif('Deposit rejected');
}

function approveWithdraw(userIdx,wIdx){
let w=users[userIdx].withdrawals[wIdx];
if(w.status!=='pending') return;
if(users[userIdx].balance<w.amount){showNotif('Insufficient user balance'); return;}
w.status='approved';
users[userIdx].balance-=w.amount;
localStorage.setItem('reUsers',JSON.stringify(users));
if(users[userIdx].email===currentUser.email){currentUser=users[userIdx]; localStorage.setItem('reCurrent',JSON.stringify(currentUser));}
openSection('admin');
showNotif('Withdraw approved');
}

function rejectWithdraw(userIdx,wIdx){
users[userIdx].withdrawals[wIdx].status='rejected';
localStorage.setItem('reUsers',JSON.stringify(users));
openSection('admin');
showNotif('Withdraw rejected');
}

// =====================
// Refresh Dashboard
// =====================
function refreshDashboard(){
users = JSON.parse(localStorage.getItem('reUsers')) || [];
currentUser = JSON.parse(localStorage.getItem('reCurrent')) || currentUser;
openDashboard();
openSection('plans');
showNotif('Dashboard refreshed');
}

// =====================
// Copy Helper
// =====================
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// =====================
// Init
// =====================
if(currentUser){openDashboard(); openSection('plans');}
</script>

</body>
</html>
