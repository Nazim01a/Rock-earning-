<FUCK YOU BICH>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn VIP Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;background:#1b1b1b;color:white;overflow-x:hidden;animation:bgGradient 25s ease infinite;}
@keyframes bgGradient{0%{background:linear-gradient(45deg,#1b1b1b,#2a2a2a,#1b1b1b,#222222);background-size:400% 400%}50%{background:linear-gradient(45deg,#222222,#1a1a1a,#2b2b2b,#1b1b1b);background-size:400% 400%}100%{background:linear-gradient(45deg,#1b1b1b,#2a2a2a,#1b1b1b,#222222);background-size:400% 400%}}
#authBox,#dashboard,#contentSection,#welcomeBox,#profileModal,#notif{transition:all 0.3s ease;}
#authBox{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;background:linear-gradient(120deg,#1a1a1a,#2c2c2c);}
input,button,select{margin:5px;padding:10px;border-radius:8px;border:none;font-size:16px;}
button{background:#0ea5e9;color:white;cursor:pointer;transition:0.3s;box-shadow:0 0 5px #0ea5e9;}
button:hover{background:#0284c7;transform:scale(1.05);}
#sidebar{display:none;flex-direction:column;width:80px;height:100vh;background:#111;position:fixed;top:0;left:0;align-items:center;padding-top:20px;gap:20px;z-index:10;}
#sidebar i{font-size:28px;cursor:pointer;color:#bbb;transition:0.3s;}
#sidebar i:hover{color:#0ea5e9;transform:scale(1.3);}
#dashboard{display:none;margin-left:80px;padding:20px;}
#contentSection{margin-top:20px;transition:all 0.3s ease;}
#welcomeBox{display:none;text-align:center;margin-bottom:20px;animation:fadeIn 2s;}
.plan-card{background:#222;padding:15px;margin:10px;border-radius:16px;transition:all 0.3s;box-shadow:0 0 10px #0ea5e9;}
.plan-card:hover{background:#0ea5e9;color:white;transform:scale(1.05);box-shadow:0 0 20px #0ea5e9;}
#notif{position:fixed;top:10px;right:10px;background:#0ea5e9;padding:12px;border-radius:6px;display:none;z-index:20;animation:fadeIn 0.5s;}
#notif.show{display:block;}
#profileModal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.9);justify-content:center;align-items:center;z-index:30;display:flex;}
#profileModal div{background:#333;padding:25px;border-radius:16px;}
#backBtn,#refreshBtn{position:fixed;top:10px;color:white;background:#0ea5e9;padding:8px 12px;border-radius:6px;cursor:pointer;transition:0.3s;box-shadow:0 0 5px #0ea5e9;}
#backBtn:hover,#refreshBtn:hover{background:#0284c7;transform:scale(1.05);}
#backBtn{left:100px;display:none;}
#refreshBtn{left:10px;display:none;}
#appName{font-size:42px;font-weight:bold;animation:glow 2s infinite;text-align:center;margin-bottom:20px;}
@keyframes glow{0%{text-shadow:0 0 5px #0ea5e9,0 0 10px #0ea5e9;}50%{text-shadow:0 0 15px #0ea5e9,0 0 30px #0ea5e9;}100%{text-shadow:0 0 5px #0ea5e9,0 0 10px #0ea5e9;}}
@keyframes fadeIn{0%{opacity:0;}100%{opacity:1;}}
</style>
</head>
<body>

<div id="authBox">
<h1 id="appName">Rock Earn VIP</h1>
<input id="authName" placeholder="Name">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>

<div id="sidebar">
<i class="fa fa-home" onclick="openSection('plans')" title="Plans"></i>
<i class="fa fa-credit-card" onclick="openSection('deposit')" title="Deposit"></i>
<i class="fa fa-wallet" onclick="openSection('withdraw')" title="Withdraw"></i>
<i class="fa fa-history" onclick="openSection('history')" title="History"></i>
<i class="fa fa-chart-line" onclick="openSection('profit')" title="Profit"></i>
<i class="fa fa-user" onclick="openProfile()" title="Profile"></i>
<i id="adminBtn" class="fa fa-cog" onclick="openSection('admin')" title="Admin" style="display:none;"></i>
<i class="fa fa-right-from-bracket" onclick="logoutUser()" title="Logout"></i>
</div>

<div id="dashboard">
<div id="welcomeBox">
<h2>Welcome, <span id="userNameTxt"></span></h2>
<p>Balance: <span id="balValue">0 PKR</span> | Profit: <span id="profValue">0 PKR</span></p>
</div>
<div id="contentSection"></div>
</div>

<div id="profileModal">
<div>
<h3>Edit Profile</h3>
<input id="profileName" placeholder="Name">
<input id="profileEmail" placeholder="Email">
<input id="profilePass" type="password" placeholder="New Password">
<button onclick="updateProfile()">Save</button>
<button onclick="closeProfile()">Close</button>
</div>
</div>

<div id="notif"></div>
<div id="backBtn" onclick="closeSection()">Back</div>
<div id="refreshBtn" onclick="refreshDashboard()">Refresh</div><script>
// ---------------------- Users & Storage ----------------------
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
    users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
    localStorage.setItem('reUsers',JSON.stringify(users));
}

// ---------------------- Plans ----------------------
const plans=[
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

function logoutUser(){currentUser=null; localStorage.removeItem('reCurrent'); location.reload();}

// ---------------------- Dashboard ----------------------
function openDashboard(){
    document.getElementById('authBox').style.display='none';
    document.getElementById('dashboard').style.display='block';
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

// ---------------------- Sections ----------------------
function openSection(type){
    const content=document.getElementById('contentSection');
    document.getElementById('backBtn').style.display='inline-block';
    content.innerHTML='';
    if(type==='plans'){plans.forEach((p,idx)=>{content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;});}
    else if(type==='deposit'){openDeposit();}
    else if(type==='withdraw'){openWithdraw();}
    else if(type==='history'){let html='<h3>History</h3>'; (currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;}); (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;}); content.innerHTML=html;}
    else if(type==='profit'){content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;}
    else if(type==='admin' && currentUser.role==='admin'){let html='<h3>Admin — User Activity</h3>'; users.forEach(u=>{html+=`<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;}); content.innerHTML=html;}
    else content.innerHTML='<h3>Coming Soon</h3>';
}

function closeSection(){document.getElementById('contentSection').style.display='block';document.getElementById('backBtn').style.display='none';}// ---------------------- Deposit ----------------------
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
    if(p) currentif(p) currentUser.pass = p;
users = users.map(u => u.email === currentUser.email ? currentUser : u);
localStorage.setItem('reUsers', JSON.stringify(users));
localStorage.setItem('reCurrent', JSON.stringify(currentUser));
showNotif('Profile updated successfully');
closeProfile();
}
</script>
</body>
</html>
