<lol>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
/* ---------------------- Body & Background Animation ---------------------- */
body{margin:0;font-family:'Segoe UI',sans-serif;color:white;overflow-x:hidden;
background: linear-gradient(120deg,#1b1b1b,#2c2c2c);
animation:bgAnim 15s ease-in-out infinite alternate;}
@keyframes bgAnim{
0%{background: linear-gradient(120deg,#1b1b1b,#2c2c2c);}
50%{background: linear-gradient(120deg,#1b1b1b,#3c3c4c);}
100%{background: linear-gradient(120deg,#1b1b1b,#2c2c2c);}
}

/* Rock Earn Logo Animation */
#appLogo{font-size:2.5rem;font-weight:bold;text-align:center;
animation:logoBounce 2s infinite alternate;text-shadow:0 0 10px #00f,0 0 20px #0ff;color:#0ff;margin:20px 0;}
@keyframes logoBounce{0%{transform:scale(1);color:#0ff;}50%{transform:scale(1.1);color:#0fa;}100%{transform:scale(1);color:#0ff;}}

/* Sidebar & Buttons */
#sidebar{display:none;flex-direction:column;width:220px;background:#111;padding:15px;}
#sidebar button{margin:5px 0;padding:10px;border:none;background:#222;color:#0ff;cursor:pointer;transition:0.3s;border-radius:8px;}
#sidebar button:hover{background:#0aa;color:white;transform:scale(1.05);}

/* Welcome Box */
#welcomeBox{display:none;text-align:center;margin:20px;}
#welcomeBox h2{font-size:1.5rem;animation:welcomeAnim 2s infinite alternate;color:#0ff;}
@keyframes welcomeAnim{0%{color:#0ff;text-shadow:0 0 10px #00f;}50%{color:#0fa;text-shadow:0 0 15px #0ff;}100%{color:#0ff;text-shadow:0 0 10px #00f;}}

/* Plan Cards */
.plan-card{background:#222;border-radius:12px;padding:15px;margin:10px;
box-shadow:0 0 10px #0ff inset,0 0 15px #0aa;transition:0.3s;display:inline-block;}
.plan-card:hover{transform:scale(1.05);box-shadow:0 0 15px #0ff inset,0 0 20px #0fa;}

/* Notification */
#notif{position:fixed;top:20px;right:20px;background:#0aa;padding:10px 20px;border-radius:8px;display:none;font-weight:bold;}
#notif.show{display:block;animation:notifAnim 0.5s ease-in-out;}
@keyframes notifAnim{0%{opacity:0;transform:translateX(50px);}100%{opacity:1;transform:translateX(0);}}

/* Back Button */
#backBtn{display:none;padding:8px 12px;background:#0aa;color:white;border:none;border-radius:8px;cursor:pointer;position:fixed;top:80px;left:10px;z-index:999;}

/* Profile Modal */
#profileModal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;
background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:9999;flex-direction:column;}
#profileModal input{margin:5px;padding:8px;border-radius:6px;border:none;width:200px;}
#profileModal button{margin-top:10px;padding:8px 12px;border:none;border-radius:6px;background:#0aa;color:white;cursor:pointer;}

/* Content Section */
#contentSection{padding:20px;text-align:center;}
</style>
<body>
<div id="appLogo">Rock Earn</div>

<div id="authBox">
<h2>Login / Sign Up</h2>
<input id="authName" placeholder="Name" style="display:block;margin:10px auto;">
<input id="authEmail" placeholder="Email" style="display:block;margin:10px auto;">
<input id="authPass" placeholder="Password" type="password" style="display:block;margin:10px auto;">
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>

<div id="sidebar">
<button onclick="openSection('plans')">Plans</button>
<button onclick="openSection('deposit')">Deposit</button>
<button onclick="openWithdraw()">Withdraw</button>
<button onclick="openSection('profit')">Profit</button>
<button onclick="openSection('history')">History</button>
<button id="adminBtn" onclick="openSection('admin')" style="display:none;">Admin</button>
<button onclick="openProfile()">Profile</button>
<button onclick="logoutUser()">Logout</button>
</div>

<div id="welcomeBox">
<h2 id="userNameTxt">Welcome User</h2>
<p>Balance: <span id="balValue">0 PKR</span> | Profit: <span id="profValue">0 PKR</span></p>
<button id="refreshBtn" onclick="refreshDashboard()" style="display:none;">Refresh</button>
</div>

<div id="contentSection"></div>
<button id="backBtn" onclick="closeSection()">Back</button>
<div id="notif"></div>

<!-- Profile Modal -->
<div id="profileModal">
<input id="profileName" placeholder="Name">
<input id="profileEmail" placeholder="Email">
<input id="profilePass" type="password" placeholder="Password (optional)">
<button onclick="updateProfile()">Update</button>
<button onclick="closeProfile()">Close</button>
</div>

<script>
// ---------------------- Users & Storage ----------------------
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
localStorage.setItem('reUsers',JSON.stringify(users));
}

// Plans
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
function showNotif(msg){const el=document.getElementById('notif');el.innerText=msg;el.classList.add('show');setTimeout(()=>el.classList.remove('show'),3000);}

// ---------------------- Auth ----------------------
function signupUser(){
const n=document.getElementById('authName').value.trim();
const e=document.getElementById('authEmail').value.trim().toLowerCase();
const p=document.getElementById('authPass').value;
if(!n||!e||!p){showNotif('Fill all fields');return;}
if(users.find(u=>u.email===e)){showNotif('Email exists');return;}
let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));
currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
openDashboard();showNotif('Account created & logged in');}

function loginUser(){
const e=document.getElementById('authEmail').value.trim().toLowerCase();
const p=document.getElementById('authPass').value;
const u=users.find(x=>x.email===e && x.pass===p);
if(!u){showNotif('Invalid credentials');return;}
currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
openDashboard();showNotif('Welcome '+currentUser.name.split(' ')[0]);}

function logoutUser(){
currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

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

// ---------------------- Open Section ----------------------
function openSection(type){
const content=document.getElementById('contentSection');
document.getElementById('backBtn').style.display='inline-block';
content.innerHTML='';
if(type==='plans'){
plans.forEach((p,idx)=>{
content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
});
}else if(type==='deposit'){openDeposit();}
else if(type==='withdraw'){openWithdraw();}
else if(type==='profit'){content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;}
else if(type==='history'){
let html='<h3>History</h3>';
(currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;});
(currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;});
content.innerHTML=html;}
else if(type==='admin' && currentUser.role==='admin'){
let html='<h3>Admin — User Activity</h3>';
users.forEach(u=>{
html+=`<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;
});
content.innerHTML=html;}
else{content.innerHTML='<h3>Coming Soon</h3>';}
}

// ---------------------- Deposit / Buy Plan ----------------------
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
</div>`;}

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
<button onclick="confirmWithdraw()">Submit Withdraw</button>`;}

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
document.getElementById('profileEmail').value=currentUser.email;}
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

// ---------------------- Close Section ----------------------
function closeSection(){
document.getElementById('contentSection').style.display='block';
document.getElementById('backBtn').style.display='none';
}

// ---------------------- Init ----------------------
if(currentUser) openDashboard();
</script>
</body>
</html>
