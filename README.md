<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;color:white;background:#000;overflow-x:hidden;animation:bgAnim 20s linear infinite;}
@keyframes bgAnim{0%{background:#000;}50%{background:#0ff;}100%{background:#000;}}
#authBox{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh;animation:fadeIn 1s;}
#sidebar{display:none;flex-direction:column;width:220px;position:fixed;top:0;left:0;height:100%;background:rgba(0,0,0,0.9);padding:10px;z-index:999;}
#sidebar button{margin:5px 0;padding:10px;background:rgba(0,255,255,0.05);border:none;color:#0ff;border-radius:8px;transition:0.3s;}
#sidebar button:hover{background:rgba(0,255,255,0.2);transform:scale(1.05);}
#welcomeBox{display:none;padding:20px;text-align:center;animation:fadeIn 1s;}
#contentSection{display:none;padding:20px;margin-left:230px;}
#backBtn,#refreshBtn{display:none;position:fixed;top:10px;padding:8px 15px;border:none;border-radius:8px;cursor:pointer;background:#0ff;color:black;z-index:1000;}
#backBtn{left:10px;} #refreshBtn{right:10px;}
#notif{position:fixed;top:20px;right:20px;background:#0ff;color:black;padding:10px 20px;border-radius:8px;opacity:0;transition:0.3s;z-index:1000;}
#notif.show{opacity:1;}
#profileModal{display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,0,0,0.95);padding:20px;border-radius:12px;z-index:10000;}
.plan-card,.admin-card{background:rgba(0,255,255,0.05);padding:10px;margin:10px 0;border-radius:10px;transition:0.3s;}
.plan-card:hover,.admin-card:hover{background:rgba(0,255,255,0.15);transform:scale(1.03);}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@keyframes textGlow{0%{text-shadow:0 0 5px #0ff;}100%{text-shadow:0 0 20px #0ff;}}
#rockEarnName{animation:textGlow 2s infinite alternate;}
input,select{margin:5px 0;padding:5px;border-radius:5px;border:none;color:black;}
button{cursor:pointer;}
</style>
</head>
<body>

<div id="authBox">
    <h1 id="rockEarnName" style="font-size:3em;">Rock Earn</h1>
    <input id="authName" placeholder="Full Name" />
    <input id="authEmail" placeholder="Email" />
    <input id="authPass" placeholder="Password" type="password"/>
    <button onclick="signupUser()">Sign Up</button>
    <button onclick="loginUser()">Login</button>
</div>

<div id="sidebar">
    <button onclick="openSection('plans')"><i class="fa-solid fa-list"></i> Plans</button>
    <button onclick="openSection('deposit')"><i class="fa-solid fa-wallet"></i> Deposit</button>
    <button onclick="openSection('withdraw')"><i class="fa-solid fa-arrow-up-right-from-square"></i> Withdraw</button>
    <button onclick="openSection('profit')"><i class="fa-solid fa-coins"></i> Profit</button>
    <button onclick="openSection('history')"><i class="fa-solid fa-clock-rotate-left"></i> History</button>
    <button onclick="openProfile()"><i class="fa-solid fa-user"></i> Profile</button>
    <button id="adminBtn" style="display:none;" onclick="openSection('admin')"><i class="fa-solid fa-shield"></i> Admin</button>
</div>

<div id="welcomeBox">
    <h2 id="userNameTxt" style="animation:textGlow 2s infinite alternate;">Welcome</h2>
    <p>Balance: <span id="balValue">0 PKR</span> | Profit: <span id="profValue">0 PKR</span></p>
</div>

<button id="backBtn" onclick="closeSection()">Back</button>
<button id="refreshBtn" onclick="refreshDashboard()">Refresh</button>
<div id="notif"></div>
<div id="contentSection"><div id="sectionContent"></div></div>

<div id="profileModal">
    <h3>Edit Profile</h3>
    <input id="profileName" placeholder="Name" />
    <input id="profileEmail" placeholder="Email" />
    <input id="profilePass" placeholder="Password" type="password"/>
    <button onclick="updateProfile()">Save</button>
    <button onclick="closeProfile()">Close</button>
</div>

<script>
// ---------------- Users & Storage ----------------
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
    users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
    localStorage.setItem('reUsers',JSON.stringify(users));
}

// Plans
const plans = [
{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},
{name:'Advanced',amount:1500,daily:250,days:25},{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30},
{name:'Platinum',amount:5000,daily:950,days:40},{name:'Diamond',amount:7000,daily:1350,days:50},{name:'VIP',amount:10000,daily:2000,days:60},
{name:'Elite',amount:12000,daily:2400,days:60},{name:'Master',amount:15000,daily:3000,days:70},{name:'Ultimate',amount:18000,daily:3500,days:75},
{name:'Legend',amount:20000,daily:4000,days:80},{name:'Supreme',amount:25000,daily:5000,days:90},{name:'Titan',amount:30000,daily:6000,days:100}
];

// ---------------- Notifications ----------------
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }

// ---------------- Auth ----------------
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

// ---------------- Dashboard ----------------
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
function refreshDashboard(){if(currentUser){currentUser=users.find(u=>u.email===currentUser.email); localStorage.setItem('reCurrent',JSON.stringify(currentUser)); document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR'; document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR'; showNotif('Dashboard refreshed'); closeSection();}}

// ---------------- Sections, Deposit & Withdraw ----------------
function openSection(type){
    const content=document.getElementById('sectionContent');
    document.getElementById('backBtn').style.display='inline-block';
    content.innerHTML='';
    if(type==='plans'){
        plans.forEach((p,idx)=>{
            content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
        });
    } else if(type==='deposit'){content.innerHTML='<h3>Deposit Section</h3>';}
    else if(type==='withdraw'){openWithdraw();}
    else if(type==='profit'){content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;}
    else if(type==='history'){let html='<h3>History</h3>'; (currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;}); (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;}); content.innerHTML=html;}
    else if(type==='admin' && currentUser.role==='admin'){let html='<h3>Admin — User Activity</h3>'; users.forEach(u=>{html+=`<div class="admin-card">${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;}); content.innerHTML=html;}
    else {content.innerHTML='<h3>Coming Soon</h3>';}
}
function closeSection(){document.getElementById('contentSection').style.display='block'; document.getElementById('backBtn').style.display='none';}

// Deposit / Buy
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
    currentUser.profit += daily; 
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Deposit confirmed — balance updated');
    openSection('plans');
}

// Withdraw
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

// Copy
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// Profile
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

// Init
if(currentUser) openDashboard();
</script>
</body>
</html>
