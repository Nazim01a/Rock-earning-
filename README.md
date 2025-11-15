<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
/* ---------------------- */
/* Basic Styles & Background Animation */
/* ---------------------- */
body {
    font-family:'Segoe UI',sans-serif;
    margin:0; padding:0; 
    color:white; 
    background: linear-gradient(120deg, #1a1a1a, #3d1a1a, #1a1a1a, #3d1a1a);
    background-size: 400% 400%;
    animation: gradientBG 20s ease infinite;
}

@keyframes gradientBG {
  0%{background-position:0% 50%;}
  50%{background-position:100% 50%;}
  100%{background-position:0% 50%;}
}

/* Glass style cards */
.glass {
    background: rgba(255,255,255,0.05);
    border-radius: 12px;
    padding: 12px;
    box-shadow: 0 0 15px rgba(0,0,0,0.3);
    backdrop-filter: blur(6px);
}

/* Buttons */
button {
    cursor:pointer;
    padding:6px 12px;
    border:none;
    border-radius:6px;
    background: #ff7700;
    color:white;
    transition:0.3s;
}
button:hover {background:#ff5500;}

/* Sidebar */
#sidebar {
    position:fixed;
    top:0; left:0;
    width:80px;
    height:100%;
    display:none;
    flex-direction:column;
    background: rgba(0,0,0,0.5);
    padding-top:20px;
    z-index:10;
}
#sidebar i {
    font-size:24px;
    color:white;
    margin:20px 0;
    text-align:center;
    cursor:pointer;
    transition:0.3s;
}
#sidebar i:hover {color:#ff7700; transform:scale(1.2);}

/* Content Section */
#contentSection {margin-left:100px; padding:20px;}
#plansSection .plan-card {margin:10px 0; padding:10px; border-radius:10px; background: rgba(255,255,255,0.05);}
#notif {position:fixed;top:10px;left:50%;transform:translateX(-50%);padding:10px 20px;border-radius:8px;background:#ff7700;color:white;display:none;z-index:50;}
#notif.show {display:block;animation:fadeInOut 3s forwards;}
@keyframes fadeInOut {0%{opacity:0}10%{opacity:1}90%{opacity:1}100%{opacity:0}}

#authBox {padding:50px;text-align:center;}
input {padding:6px; margin:5px; border-radius:6px; border:none;}
#backBtn, #refreshBtn {position:fixed;top:20px; right:20px; display:none;}
#refreshBtn {right:70px;}
</style>
</head>
<body>

<!-- Notification -->
<div id="notif"></div>

<!-- Login / Signup -->
<div id="authBox">
    <h1 style="font-size:32px; animation:colorChange 4s infinite alternate;">Welcome to Rock Earn</h1>
    <input id="authName" placeholder="Your Name"><br>
    <input id="authEmail" placeholder="Email"><br>
    <input id="authPass" placeholder="Password" type="password"><br>
    <button onclick="signupUser()">Sign Up</button>
    <button onclick="loginUser()">Login</button>
</div>

<!-- Sidebar -->
<div id="sidebar">
    <i class="fa fa-home" title="Dashboard" onclick="openSection('dashboard')"></i>
    <i class="fa fa-list" title="Plans" onclick="openSection('plans')"></i>
    <i class="fa fa-money-bill" title="Deposit" onclick="openSection('deposit')"></i>
    <i class="fa fa-wallet" title="Withdraw" onclick="openSection('withdraw')"></i>
    <i class="fa fa-chart-line" title="Profit" onclick="openSection('profit')"></i>
    <i class="fa fa-history" title="History" onclick="openSection('history')"></i>
    <i class="fa fa-user" title="Profile" onclick="openProfile()"></i>
    <i class="fa fa-user-shield" id="adminBtn" title="Admin" onclick="openSection('admin')" style="display:none;"></i>
</div>

<!-- Welcome Box -->
<div id="welcomeBox" style="display:none; text-align:center; padding:20px;">
    <h2 id="userNameTxt"></h2>
    <div class="glass" style="display:inline-block; padding:10px; margin-top:10px;">
        Balance: <span id="balValue"></span><br>
        Profit: <span id="profValue"></span>
    </div>
</div>

<!-- Back & Refresh Buttons -->
<button id="backBtn" onclick="closeSection()">Back</button>
<button id="refreshBtn" onclick="refreshDashboard()">Refresh</button>

<!-- Content Section -->
<div id="contentSection"></div>

<!-- Profile Modal -->
<div id="profileModal" style="display:none; position:fixed; top:10%; left:50%; transform:translateX(-50%); background:rgba(0,0,0,0.7); padding:20px; border-radius:12px;">
    <h3>Profile</h3>
    <input id="profileName" placeholder="Name"><br>
    <input id="profileEmail" placeholder="Email"><br>
    <input id="profilePass" placeholder="New Password" type="password"><br>
    <button onclick="updateProfile()">Update</button>
    <button onclick="closeProfile()">Close</button>
</div>

<!-- ---------------------- -->
<!-- JS PART (Full Authentication + Dashboard + Plans + Deposit/Withdraw + Admin) -->
<!-- ---------------------- -->
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

// ----------------------
// Notifications
// ----------------------
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }

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
// Dashboard & Sections
// ----------------------
function openDashboard(){
    document.getElementById('authBox').style.display='none';
    document.getElementById('sidebar').style.display='flex';
    document.getElementById('welcomeBox').style.display='block';
    document.getElementById('refreshBtn').style.display='inline-block';
    document.getElementById('userNameTxt').innerText=currentUser.name.split(' ')[0];
    document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
    document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
    if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
    document.getElementById('backBtn').style.display='none';
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
    document.getElementById('backBtn').style.display='inline-block';
    content.innerHTML='';

    if(type==='plans'){
        plans.forEach((p,idx)=>{
            content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
        });
    } else if(type==='deposit'){
        content.innerHTML='<h3>Deposit Section</h3>';
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

function closeSection(){
    document.getElementById('contentSection').style.display='block';
    document.getElementById('backBtn').style.display='none';
}

// ----------------------
// Deposit / Buy Plan
// ----------------------
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
    const content=document.getElementById('sectionContent');
    document.getElementById('contentSection').style.display='block';
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
    currentUser.profit += daily; // first day profit auto
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Deposit confirmed — balance updated');
    openSection('plans');
}

// ----------------------
// Withdraw
// ----------------------
function openWithdraw(){
    const content=document.getElementById('sectionContent');
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

// ----------------------
// Copy Helper
// ----------------------
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// ----------------------
// Profile Modal
// ----------------------
function openProfile(){
    document.getElementById('profileModal').style.display='block';
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

// ----------------------
// Init on load
// ----------------------
if(currentUser) openDashboard();
</script>

</body>
</html>
