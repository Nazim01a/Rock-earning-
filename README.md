<ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Pro — Premium Gold Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0a0f1f,#1b1f3b);color:white;animation:bgAnim 15s infinite alternate;}
@keyframes bgAnim{0%{background:linear-gradient(120deg,#0a0f1f,#1b1f3b);}50%{background:linear-gradient(120deg,#1b1f3b,#0a0f1f);}100%{background:linear-gradient(120deg,#0a0f1f,#1b1f3b);}}
.card{background:rgba(255,255,255,0.05);border-radius:20px;padding:20px;backdrop-filter:blur(15px);box-shadow:0 0 30px #0ff6;transition:0.3s;transform:translateY(0);animation:fadeIn 0.8s ease forwards;}
@keyframes fadeIn{0%{opacity:0;transform:translateY(20px);}100%{opacity:1;transform:translateY(0);}}
.card:hover{transform:scale(1.03);box-shadow:0 0 35px #0ff;}
.btn{background:#4f46e5;padding:10px 16px;border-radius:12px;font-weight:700;transition:0.2s;}
.btn:hover{background:#4338ca;transform:scale(1.05);}
input,select{padding:12px;border-radius:12px;width:100%;margin-bottom:12px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 10px #0ff;}
.icon-btn{display:flex;align-items:center;gap:10px;font-weight:600;transition:0.3s;}
.icon-btn:hover{transform:scale(1.05);}
#sidePanel{position:fixed;top:0;right:-420px;width:400px;height:100vh;background:rgba(15,23,42,0.95);padding:20px;transition:0.4s;overflow-y:auto;z-index:999;}
#sidePanel.active{right:0;}
.plan-list{margin-top:20px;}
.plan-item{border:1px solid #0ff;padding:12px;margin-bottom:10px;border-radius:12px;transition:0.2s;animation:planFade 0.5s ease forwards;}
@keyframes planFade{0%{opacity:0;transform:translateX(-20px);}100%{opacity:1;transform:translateX(0);}}
.plan-item:hover{box-shadow:0 0 15px #0ff;transform:scale(1.02);}
.header-logo{font-size:36px;font-weight:800;text-align:center;background:linear-gradient(90deg,#FFD700,#FF8C00);-webkit-background-clip:text;-webkit-text-fill-color:transparent;animation:logoGlow 2s infinite alternate;}
@keyframes logoGlow{0%{text-shadow:0 0 5px #FFD700;}50%{text-shadow:0 0 20px #FFD700;}100%{text-shadow:0 0 5px #FFD700;}}
.logout-btn{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);width:200px;}
</style>
</head>
<body class="p-4"><!-- HEADER --><header class="text-center py-6">
  <div class="header-logo">🚀 Rock Earn Premium Gold</div>
  <p class="opacity-70">Since 2018 • Crypto FinTech • Partnered with Binance</p>
</header><!-- AUTH --><div id="authBox" class="card mb-6">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" />
  <input id="authEmail" placeholder="Email" />
  <input id="authPass" type="password" placeholder="Password" />
  <button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
  <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div><!-- DASHBOARD ICONS --><div id="welcomeBox" style="display:none;" class="card mb-4 p-4 text-center"></div>
<div id="dashboard" style="display:none;" class="grid grid-cols-3 gap-4 mb-6">
  <button class="icon-btn btn" onclick="openPanel('profile')"><i class="fa fa-user"></i> Profile</button>
  <button class="icon-btn btn bg-green-600" onclick="openPanel('deposit')"><i class="fa fa-wallet"></i> Deposit</button>
  <button class="icon-btn btn bg-yellow-600" onclick="openPanel('withdraw')"><i class="fa fa-money-bill"></i> Withdraw</button>
  <button class="icon-btn btn bg-purple-600" onclick="openPanel('activity')"><i class="fa fa-list"></i> Activity</button>
  <button class="icon-btn btn bg-pink-600" onclick="openPanel('plans')"><i class="fa fa-gem"></i> Plans</button>
  <button class="icon-btn btn bg-gray-600" onclick="openPanel('company')"><i class="fa fa-building"></i> Company</button>
  <button class="icon-btn btn bg-red-600" onclick="openPanel('settings')"><i class="fa fa-cog"></i> Settings</button>
  <button class="icon-btn btn bg-blue-600" onclick="openPanel('referral')"><i class="fa fa-link"></i> Referral</button>
  <button class="icon-btn btn bg-indigo-600" onclick="openPanel('leaderboard')"><i class="fa fa-trophy"></i> Leaderboard</button>
  <button class="icon-btn btn bg-teal-600" onclick="openPanel('profit')"><i class="fa fa-chart-line"></i> Daily Profit</button>
  <button class="icon-btn btn bg-orange-600" onclick="openPanel('support')"><i class="fa fa-headset"></i> Support</button>
  <button class="icon-btn btn bg-cyan-600" onclick="openPanel('faq')"><i class="fa fa-question-circle"></i> FAQ</button>
</div><!-- SIDE PANEL --><div id="sidePanel"></div><!-- LOGOUT BUTTON FIXED BOTTOM CENTER --><button onclick="logoutUser()" class="btn bg-red-600 logout-btn"><i class="fa fa-sign-out-alt"></i> Logout</button>

<script>
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(currentUser){openDashboard();}
function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return alert('Fill all fields');if(!validateEmail(e))return alert('Invalid email');if(users.find(u=>u.email===e))return alert('Email already registered');let u={name:n,email:e,pass:p,plans:[],referrals:[],balance:0,profit:0,notifications:true};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return alert('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('welcomeBox').style.display='block';document.getElementById('dashboard').style.display='grid';document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💎</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
let plans=[{name:'Ulta Pro',amount:500, daily:80},{name:'Premium',amount:1000,daily:180},{name:'Gold',amount:2500,daily:450},{name:'Platinum',amount:5000,daily:950},{name:'Diamond',amount:7000,daily:1350},{name:'Elite',amount:10000,daily:2200},{name:'Mega Booster',amount:15000,daily:3500,coming:true},{name:'Ultra Pro',amount:20000,daily:4800,coming:true},{name:'Crypto Miner',amount:30000,daily:6500,coming:true}];
function openPanel(type,amount=0,name=''){let panel=document.getElementById('sidePanel');panel.classList.add('active');panel.innerHTML='';switch(type){case'plans':panel.innerHTML='<h2>All Plans</h2><div class="plan-list">'+plans.map(p=>`<div class='plan-item'><b>${p.name}</b> - ${p.amount} PKR - Daily Profit: ${p.daily} PKR ${p.coming?'(Coming Soon)':''}<button class='btn mt-1' onclick='buyPlan(${p.amount},"${p.name}")'>Buy Now</button></div>`).join('')+'</div>';break;case'profile':panel.innerHTML=`<h2>Your Profile</h2><p>Name: ${currentUser.name}</p><p>Email: ${currentUser.email}</p><button class='btn mt-2' onclick='copyText("https://rockearnpro.com/user/${currentUser.email}")'>Copy Profile Link</button>`;break;case'deposit':panel.innerHTML=`<h2>Deposit</h2><p>Amount Selected: <b>${amount}</b> PKR</p><p>JazzCash: 03705519562 <button class='btn' onclick='copyText("03705519562")'>Copy</button></p><p>EasyPaisa: 03379827882 <button class='btn' onclick='copyText("03379827882")'>Copy</button></p><input type='file' id='proofFile'><input type='text' placeholder='Transaction ID'><button class='btn mt-2' onclick='confirmDeposit()'>Submit</button>`;break;case'withdraw':panel.innerHTML=`<h2>Withdraw</h2><input type='number' placeholder='Amount'><select><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number'><button class='btn mt-2'>Withdraw</button>`;break;case'activity':panel.innerHTML=`<h2>Activity History</h2><p>• Deposit: 500 PKR</p><p>• Withdraw: 300 PKR</p><p>• Plan Bought: Basic Plan</p>`;break;case'company':panel.innerHTML=`<h2>Company Details</h2><p>Since: <b>2018</b></p><p>Industry: <b>Crypto & FinTech</b></p><p>Partner: <b>Binance</b></p><p>Address: <b>1234 Crypto Avenue, San Francisco, California, USA</b></p><p>Email: support@rockearnpro.com</p>`;break;case'settings':panel.innerHTML=`<h2>Settings</h2><input type='text' placeholder='Change Name'><input type='password' placeholder='Change Password'><button class='btn mt-2'>Save</button>`;break;case'referral':panel.innerHTML=`<h2>Referral</h2><p>Share your referral link:</p><input type='text' value='https://rockearnpro.com?ref=${currentUser.email}' readonly><button class='btn mt-2' onclick='copyText("https://rockearnpro.com?ref=${currentUser.email}")'>Copy Link</button>`;break;case'leaderboard':panel.innerHTML=`<h2>Leaderboard</h2><p>• User1 - 10,000 PKR</p><p>• User2 - 9,500 PKR</p>`;break;case'profit':panel.innerHTML=`<h2>Daily Profit</h2><p>Total Daily Profit: ${currentUser.profit} PKR</p>`;break;case'support':panel.innerHTML=`<h2>Support</h2><p>Contact us at support@rockearnpro.com</p>`;break;case'faq':panel.innerHTML=`<h2>FAQ</h2><p>Q1: How to deposit?</p><p>A1: Use JazzCash or EasyPaisa.</p>`;break;default:panel.innerHTML='';}}
function buyPlan(amount,name){currentUser.plans.push({name:name,amount:amount});currentUser.balance+=amount;localStorage.setItem('reCurrent',JSON.stringify(currentUser));openPanel('deposit',amount,name);}
function copyText(val){navigator.clipboard.writeText(val);alert('Copied: '+val);}
function confirmDeposit(){alert('Deposit confirmed!');}
</script></body>
</html>
