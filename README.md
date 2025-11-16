<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{
    font-family:'Segoe UI',sans-serif;
    margin:0; padding:0;
    background: linear-gradient(120deg,#1b1b1b,#111);
    color:white;
    overflow-x:hidden;
    animation:bgAnim 20s linear infinite alternate;
}
@keyframes bgAnim{
    0%{background:linear-gradient(120deg,#1b1b1b,#111);}
    50%{background:linear-gradient(120deg,#111,#1b1b1b);}
    100%{background:linear-gradient(120deg,#1b1b1b,#111);}
}
#authBox{display:flex;flex-direction:column;justify-content:center;align-items:center;height:100vh;animation:fadeIn 1s;}
input,button{padding:10px;margin:5px;border-radius:8px;border:none;}
button{background:#00bfff;color:white;cursor:pointer;transition:0.3s;}
button:hover{background:#009acd;}
#notif{position:fixed;top:20px;right:20px;background:#00bfff;padding:10px 20px;border-radius:8px;opacity:0;transition:0.3s;}
#notif.show{opacity:1;}
#sidebar{display:none;position:fixed;left:0;top:0;width:250px;height:100vh;background:#111;flex-direction:column;padding:10px;gap:10px;animation:fadeInLeft 1s;}
#sidebar button{width:100%;text-align:left;}
#welcomeBox{display:none;position:fixed;top:10px;left:260px;animation:fadeInDown 1s;color:#00bfff;}
#contentSection{margin-left:260px;margin-top:80px;padding:20px;animation:fadeInUp 1s;}
#profileModal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;animation:fadeIn 0.5s;}
.plan-card{background:#222;margin:10px;padding:15px;border-radius:10px;transition:0.3s;}
.plan-card:hover{background:#333;transform:scale(1.05);}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@keyframes fadeInLeft{from{opacity:0;transform:translateX(-50px);}to{opacity:1;transform:translateX(0);}}
@keyframes fadeInDown{from{opacity:0;transform:translateY(-50px);}to{opacity:1;transform:translateY(0);}}
@keyframes fadeInUp{from{opacity:0;transform:translateY(50px);}to{opacity:1;transform:translateY(0);}}
</style>
</head>
<body>
<div id="notif"></div>

<!-- Login / Signup -->
<div id="authBox">
<h1 class="text-3xl font-bold mb-4">Welcome to <span id="rockEarnAnim">Rock Earn</span></h1>
<input type="text" id="authName" placeholder="Your Name (Signup only)">
<input type="email" id="authEmail" placeholder="Email">
<input type="password" id="authPass" placeholder="Password">
<div>
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>
</div>

<!-- Dashboard & Sidebar -->
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

<!-- Welcome Box -->
<div id="welcomeBox">
<h2 id="userNameTxt"></h2>
<p>Balance: <span id="balValue">0</span> | Profit: <span id="profValue">0</span></p>
</div>

<!-- Content Section -->
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
// ---------------------- Users & Storage ----------------------
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

// ---------------------- Notifications ----------------------
function showNotif(msg){
    const el=document.getElementById('notif');
    el.innerText=msg;
    el.classList.add('show');
    setTimeout(()=>el.classList.remove('show'),3000);
}

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
    }
}

// ---------------------- Sections ----------------------
function openSection(type){
    const content=document.getElementById('contentSection');
    content.innerHTML='';
    if(type==='plans'){
        plans.forEach((p,idx)=>{
            content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
        });
    } else if(type==='deposit'){openDeposit();}
    else if(type==='withdraw'){openWithdraw();}
    else if(type==='profit'){content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;}
    else if(type==='history'){
        let html='<h3>History</h3>';
        (currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;});
        (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;});
        content.innerHTML=html;
    } else if(type==='admin' && currentUser.role==='admin'){
        let html='<h3>Admin — User Activity</h3>';
        users.forEach(u=>{html+=`<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;});
        content.innerHTML=html;
    } else {content.innerHTML='<h3>Coming Soon</h3>';}
}

// ---------------------- Deposit ----------------------
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
    const content=document.getElementById('contentSection');
    content.innerHTML=`<h3>Deposit — ${name}</h3>
    <p>Amount: <strong>${amount} PKR</strong></p>
    <p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
    <p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
    <p>Bank (Coming Soon)</p>
    <div style="margin-top:10px">
    <input id="depositTxn" placeholder="Transaction ID">
    <input id="depositProof" type="file"><br>
    <button onclick="confirmDeposit(${amount},'${name}',${daily},${days},${planIndex})">Submit Deposit</button>
    </div>`;
}
function confirmDeposit(amount,name,daily,days,planIndex){
    const txn=document.getElementById('depositTxn').value.trim();
    const proof=document.getElementById('depositProof').files[0];
    if(!txn||!proof){showNotif('Upload proof & enter txn'); return;}
    const deposit={plan:name,amount:amount,daily:daily,days:days,txn:txn,proof:proof.name,status:'approved',ts:Date.now()};
    currentUser.deposits=currentUser.deposits||[]; currentUser.deposits.push(deposit);
    currentUser.balance+=amount;
    currentUser.profit+=daily;
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
    currentUser.balance-=amt;
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
