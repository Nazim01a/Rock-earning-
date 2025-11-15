<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{margin:0;padding:0;font-family:'Segoe UI',sans-serif;background:#0a0a0a;color:white;overflow-x:hidden;}
#notif{position:fixed;top:20px;right:20px;background:rgba(255,255,255,0.1);padding:10px 20px;border-radius:8px;backdrop-filter:blur(8px);opacity:0;transition:0.3s;z-index:1000;}
#notif.show{opacity:1;}
#welcomeBox{text-align:center;animation:fadeIn 2s ease forwards;}
#userNameTxt,#balValue,#profValue{display:inline-block;margin:0 10px;}
#rockEarnName{font-size:2rem;font-weight:bold;background:linear-gradient(90deg,#ff0044,#ffdd00,#00ffee,#ff0044);-webkit-background-clip:text;color:transparent;animation:gradientAnim 5s linear infinite;}
#sidebar{display:flex;flex-direction:column;position:fixed;left:0;top:0;bottom:0;width:70px;background:#111;justify-content:flex-start;align-items:center;padding-top:20px;}
#sidebar button{background:none;border:none;color:white;margin:10px 0;font-size:1.2rem;cursor:pointer;transition:0.3s;position:relative;}
#sidebar button:hover{transform:scale(1.2);}
.plan-card{border:1px solid #444;padding:15px;margin:10px;border-radius:10px;background:rgba(255,255,255,0.05);backdrop-filter:blur(5px);transition:0.3s;}
.plan-card:hover{transform:scale(1.05);border-color:#ff0044;}
button{background:#ff0044;color:white;border:none;padding:8px 15px;border-radius:8px;cursor:pointer;transition:0.3s;}
button:hover{background:#ff66aa;}
@keyframes fadeIn{0%{opacity:0;}100%{opacity:1;}}
@keyframes gradientAnim{0%{background-position:0%;}50%{background-position:100%;}100%{background-position:0%;}}
#contentSection{margin-left:80px;padding:20px;}
#backBtn{position:fixed;top:20px;left:80px;padding:5px 15px;background:#222;border-radius:5px;display:none;}
#refreshBtn{position:fixed;top:20px;right:20px;padding:5px 15px;background:#222;border-radius:5px;display:none;}
#profileModal{display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,0,0,0.9);padding:20px;border-radius:10px;}
</style>
<body>
<div id="notif"></div>

<div id="authBox" style="text-align:center;margin-top:50px;">
<h1 id="rockEarnName">Rock Earn</h1>
<input id="authName" placeholder="Name"><br><br>
<input id="authEmail" placeholder="Email"><br><br>
<input id="authPass" type="password" placeholder="Password"><br><br>
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>

<div id="sidebar" style="display:none;">
<button onclick="openSection('plans')"><i class="fa-solid fa-money-bill"></i></button>
<button onclick="openSection('deposit')"><i class="fa-solid fa-credit-card"></i></button>
<button onclick="openSection('withdraw')"><i class="fa-solid fa-university"></i></button>
<button onclick="openSection('profit')"><i class="fa-solid fa-chart-line"></i></button>
<button onclick="openSection('history')"><i class="fa-solid fa-clock-rotate-left"></i></button>
<button onclick="openProfile()"><i class="fa-solid fa-user"></i></button>
<button id="adminBtn" style="display:none;" onclick="openSection('admin')"><i class="fa-solid fa-gears"></i></button>
</div>

<div id="welcomeBox" style="display:none;">
<h2>Welcome to <span id="rockEarnName">Rock Earn</span></h2>
<p>Username: <span id="userNameTxt"></span></p>
<p>Balance: <span id="balValue"></span> | Profit: <span id="profValue"></span></p>
</div>

<button id="backBtn" onclick="closeSection()">🔙 Back</button>
<button id="refreshBtn" onclick="refreshDashboard()">🔄 Refresh</button>

<div id="contentSection"></div>

<div id="profileModal">
<h3>Edit Profile</h3>
<input id="profileName" placeholder="Name"><br><br>
<input id="profileEmail" placeholder="Email"><br><br>
<input id="profilePass" type="password" placeholder="New Password"><br><br>
<button onclick="updateProfile()">Update</button>
<button onclick="closeProfile()">Close</button>
</div>

<script>
// Users & Storage
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
localStorage.setItem('reUsers',JSON.stringify(users));
}

const plans=[{name:'Basic',amount:200,daily:30,days:20},{name:'Starter',amount:500,daily:75,days:20},{name:'Pro',amount:1000,daily:180,days:20},{name:'Advanced',amount:1500,daily:250,days:25},{name:'Silver',amount:2000,daily:350,days:30},{name:'Gold',amount:3000,daily:550,days:30},{name:'Platinum',amount:5000,daily:950,days:40},{name:'Diamond',amount:7000,daily:1350,days:50},{name:'VIP',amount:10000,daily:2000,days:60},{name:'Elite',amount:12000,daily:2400,days:60},{name:'Master',amount:15000,daily:3000,days:70},{name:'Ultimate',amount:18000,daily:3500,days:75},{name:'Legend',amount:20000,daily:4000,days:80},{name:'Supreme',amount:25000,daily:5000,days:90},{name:'Titan',amount:30000,daily:6000,days:100}];

function showNotif(msg){const el=document.getElementById('notif');el.innerText=msg;el.classList.add('show');setTimeout(()=>el.classList.remove('show'),3000);}
function signupUser(){const n=document.getElementById('authName').value.trim();const e=document.getElementById('authEmail').value.trim().toLowerCase();const p=document.getElementById('authPass').value;if(!n||!e||!p){showNotif('Fill all fields');return;}if(users.find(u=>u.email===e)){showNotif('Email exists');return;}let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));openDashboard();showNotif('Account created & logged in');}
function loginUser(){const e=document.getElementById('authEmail').value.trim().toLowerCase();const p=document.getElementById('authPass').value;const u=users.find(x=>x.email===e&&x.pass===p);if(!u){showNotif('Invalid credentials');return;}currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));openDashboard();showNotif('Welcome '+currentUser.name.split(' ')[0]);}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

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
showNotif('Dashboard refreshed');closeSection();}
}

function openSection(type){
const content=document.getElementById('sectionContent');
document.getElementById('backBtn').style.display='inline-block';
content.innerHTML='';
if(type==='plans'){plans.forEach((p,idx)=>{content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;});} 
else if(type==='deposit'){content.innerHTML='<h3>Deposit Section</h3>';}
else if(type==='withdraw'){openWithdraw();}
else if(type==='profit'){content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;}
else if(type==='history'){let html='<h3>History</h3>'; (currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR</p>`;}); (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method}</p>`;}); content.innerHTML=html;}
else if(type==='admin'&&currentUser.role==='admin'){let html='<h3>Admin — User Activity</h3>'; users.forEach(u=>{html+=`<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;}); content.innerHTML=html;}
else{content.innerHTML='<h3>Coming Soon</h3>';}
}

function closeSection(){document.getElementById('contentSection').style.display='block';document.getElementById('backBtn').style.display='none';}
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
if(!txn||!proof){showNotif('Upload proof & enter txn');return;}
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
if(!name||!acc||!amt){showNotif('Fill all fields');return;}
if(amt>currentUser.balance){showNotif('Insufficient balance');return;}
currentUser.balance-=amt;
currentUser.withdrawals=currentUser.withdrawals||[];
currentUser.withdrawals.push({name:name,account:acc,amount:amt,method:method,status:'done'});
users=users.map(u=>u.email===currentUser.email?currentUser:u);
localStorage.setItem('reUsers',JSON.stringify(users));
localStorage.setItem('reCurrent',JSON.stringify(currentUser));
showNotif('Withdraw processed');
openSection('withdraw');
}

function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}
function openProfile(){document.getElementById('profileModal').style.display='block';document.getElementById('profileName').value=currentUser.name;document.getElementById('profileEmail').value=currentUser.email;}
function closeProfile(){document.getElementById('profileModal').style.display='none';}
function updateProfile(){const n=document.getElementById('profileName').value.trim();const e=document.getElementById('profileEmail').value.trim();const p=document.getElementById('profilePass').value.trim();if(n)currentUser.name=n;if(e)currentUser.email=e;if(p)currentUser.pass=p;users=users.map(u=>u.email===currentUser.email?currentUser:u);localStorage.setItem('reUsers',JSON.stringify(users));localStorage.setItem('reCurrent',JSON.stringify(currentUser));showNotif('Profile updated');}
if(currentUser)openDashboard();
</script>
</body>
</html>
