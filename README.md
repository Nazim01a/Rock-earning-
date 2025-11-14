<ROCK EARNING>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Ultra Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap');

body {
  font-family:'Orbitron',sans-serif;
  margin:0; padding:0;
  background: linear-gradient(135deg,#0b0f1a,#1c1f3b);
  color:white; overflow-x:hidden;
}

header h1 {
  font-size:3rem;
  text-align:center;
  margin-top:20px;
  background: linear-gradient(90deg,#00ffe0,#5d5fef,#ff4d4d);
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
  animation: textGlow 3s ease-in-out infinite alternate;
}

@keyframes textGlow {
  0% {text-shadow:0 0 10px #00ffe0;}
  50% {text-shadow:0 0 20px #5d5fef;}
  100% {text-shadow:0 0 25px #ff4d4d;}
}

.fixed-top {
  position:sticky; top:0;
  display:flex; justify-content:space-between; align-items:center;
  background: rgba(0,0,0,0.3); backdrop-filter:blur(10px);
  padding:10px 20px; z-index:100; border-bottom:1px solid #00ffe0;
}

#userBalance {font-weight:bold; animation:balanceGlow 2s infinite alternate;}
@keyframes balanceGlow {0%{text-shadow:0 0 5px #00ffe0;}100%{text-shadow:0 0 15px #5d5fef;}}

.neon-card {
  background: rgba(255,255,255,0.05);
  border:1px solid rgba(255,255,255,0.1);
  backdrop-filter:blur(15px);
  border-radius:20px; padding:20px;
  transition: all 0.4s ease; position:relative;
}
.neon-card:hover {
  transform:scale(1.05);
  box-shadow:0 0 25px #00ffe0,0 0 50px #5d5fef,0 0 75px #ff4d4d;
}

.glow-btn {
  background: linear-gradient(90deg,#5d5fef,#00ffe0,#ff4d4d);
  color:#fff; padding:10px 15px;
  border-radius:12px; font-weight:700; cursor:pointer;
  box-shadow:0 0 10px #5d5fef,0 0 20px #00ffe0; transition:0.3s;
  position:relative; overflow:hidden;
}
.glow-btn:hover {transform:scale(1.05); box-shadow:0 0 20px #5d5fef,0 0 40px #00ffe0,0 0 60px #ff4d4d;}

#sidePanel {
  position:fixed; top:0; right:-420px; width:400px; height:100vh;
  background:#0f172a; padding:20px; overflow-y:auto;
  transition: all 0.4s ease; z-index:999; border-left:2px solid #00ffe0;
}

#logoutBtn {
  position:fixed;bottom:20px;left:50%;transform:translateX(-50%);
  background:#ff4d4d;padding:12px 25px;border-radius:15px;cursor:pointer;
  font-weight:700; box-shadow:0 0 15px #ff4d4d; transition:0.3s;
}
#logoutBtn:hover {transform:translateX(-50%) scale(1.05); box-shadow:0 0 30px #ff4d4d;}

.confetti-piece {
  position:absolute;width:6px;height:6px;border-radius:50%;top:0;
  animation:confetti 1s ease forwards;
}
@keyframes confetti {
  0% {transform:translateY(0) rotate(0deg); opacity:1;}
  100% {transform:translateY(250px) rotate(360deg); opacity:0;}
}
</style>
</head>
<body>

<!-- AUTH -->
<div id="authBox" class="p-6 max-w-md mx-auto mt-10 bg-white/5 rounded-2xl shadow-lg">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2">
<input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2">
<input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2">
<button onclick="signupUser()" class="w-full mb-2 glow-btn">Sign Up</button>
<button onclick="loginUser()" class="w-full glow-btn">Login</button>
</div>

<script>
let users = JSON.parse(localStorage.getItem("reUsers"))||[];
let currentUser = JSON.parse(localStorage.getItem("reCurrent"))||null;
function signupUser(){
  let n=document.getElementById('authName').value.trim();
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  if(!n||!e||!p){alert('Fill all fields');return;}
  if(users.find(u=>u.email===e)){alert('Email already registered');return;}
  let u={name:n,email:e,pass:p};
  users.push(u); localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(u));
  loadDashboard();
}
function loginUser(){
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  let u=users.find(u=>u.email===e && u.pass===p);
  if(!u){alert('Invalid credentials');return;}
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(u));
  loadDashboard();
}
function loadDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('dashboard').style.display='block';
  updateWelcome();
}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function updateWelcome(){document.getElementById('welcomeUser').innerText='Welcome, '+currentUser.name+'!';}
</script>

<!-- TOP BAR -->
<div class="fixed-top">
<div id="welcomeUser">Welcome, User!</div>
<div id="userBalance">Balance: ₨0</div>
</div>

<!-- MENU -->
<div class="flex gap-4 flex-wrap justify-center mt-24">
<button onclick="openPanel('profile')" class="glow-btn">Profile</button>
<button onclick="openPanel('deposit')" class="glow-btn">Deposit</button>
<button onclick="openPanel('withdraw')" class="glow-btn">Withdraw</button>
<button onclick="openPanel('activity')" class="glow-btn">Activity</button>
<button onclick="openPanel('company')" class="glow-btn">Company</button>
<button onclick="openPanel('settings')" class="glow-btn">Settings</button>
<button onclick="openPanel('plans')" class="glow-btn">Plans</button>
</div>

<!-- PLANS -->
<section class="mt-10 text-center">
<h2 class="text-3xl font-bold mb-6">Ultra Premium Plans</h2>
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 px-4">

<!-- Active Plans -->
<div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Starter Plan</h3><p>Amount: 180 PKR</p><p>Daily Profit: 20 PKR</p><button onclick="buyPlan(180,this)" class="glow-btn mt-3">Buy Now</button></div>
<div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Basic Plan</h3><p>Amount: 500 PKR</p><p>Daily Profit: 60 PKR</p><button onclick="buyPlan(500,this)" class="glow-btn mt-3">Buy Now</button></div>
<div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Standard Plan</h3><p>Amount: 1000 PKR</p><p>Daily Profit: 130 PKR</p><button onclick="buyPlan(1000,this)" class="glow-btn mt-3">Buy Now</button></div>
<div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Premium Plan</h3><p>Amount: 2500 PKR</p><p>Daily Profit: 320 PKR</p><button onclick="buyPlan(2500,this)" class="glow-btn mt-3">Buy Now</button></div>
<div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Gold Plan</h3><p>Amount: 5000 PKR</p><p>Daily Profit: 650 PKR</p><button onclick="buyPlan(5000,this)" class="glow-btn mt-3">Buy Now</button></div>
<div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Elite Plan</h3><p>Amount: 7000 PKR</p><p>Daily Profit: 900 PKR</p><button onclick="buyPlan(7000,this)" class="glow-btn mt-3">Buy Now</button></div>

<!-- Coming Soon Plans -->
<div class="neon-card p-6 opacity-50"><h3 class="text-xl font-semibold mb-1">Mega Booster (Coming Soon)</h3><p>Amount: 10,000 PKR</p><p>Daily Profit: 1,500 PKR</p></div>
<div class="neon-card p-6 opacity-50"><h3 class="text-xl font-semibold mb-1">Ultra Pro (Coming Soon)</h3><p>Amount: 15,000 PKR</p><p>Daily Profit: 2,300 PKR</p></div>
<div class="neon-card p-6 opacity-50"><h3 class="text-xl font-semibold mb-1">Crypto Miner (Coming Soon)</h3><p>Amount: 20,000 PKR</p><p>Daily Profit: 3,500 PKR</p></div>

</div>
</section>

<!-- SIDE PANEL -->
<div id="sidePanel"><div id="closePanel" onclick="closePanel()">X</div></div>

<script>
function openPanel(type){
let panel=document.getElementById('sidePanel'); panel.style.right='0';
let html='';
switch(type){
case 'profile': html=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Profile</h2><p>Name: ${currentUser?currentUser.name:'User'}</p><p>ID: RE-928373</p><button onclick="copyText('https://rockearnpro.com/user/928373')" class="glow-btn mt-2">Copy Profile Link</button>`; break;
case 'deposit': html=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Deposit</h2><p id='depositAmount'>Amount Selected: 0 PKR</p><p>JazzCash: 03705519562 <button onclick="copyText('03705519562')" class="glow-btn mt-1">Copy</button></p><p>EasyPaisa: 03379827882 <button onclick="copyText('03379827882')" class="glow-btn mt-1">Copy</button></p><input type='file' class='mt-3'><input type='text' placeholder='Transaction ID' class='mt-3 p-2 w-full rounded bg-white/20'><button class="glow-btn mt-4 w-full">Submit</button>`; break;
case 'withdraw': html=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Withdraw</h2><input type='number' placeholder='Amount' class='w-full p-2 rounded bg-white/20 mb-3'><select class='w-full p-2 rounded bg-white/20 mb-3'><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number' class='w-full p-2 rounded bg-white/20 mb-3'><button class="glow-btn w-full">Withdraw</button>`; break;
case 'activity': html=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Activity History</h2><p>• Deposit: 500 PKR</p><p>• Withdraw: 300 PKR</p><p>• Plan Bought: Basic Plan</p>`; break;
case 'company': html=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Company</h2><p>Since: 2018</p><p>Industry: Crypto & FinTech</p><p>Partner: Binance</p><p>Address: 1234 Crypto Avenue, San Francisco, California, USA</p><p>Email: support@rockearnpro.com</p>`; break;
case 'settings': html=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Settings</h2><input type='text' placeholder='Change Name' class='w-full p-2 rounded bg-white/20 mb-3'><input type='password' placeholder='Change Password' class='w-full p-2 rounded bg-white/20 mb-3'><button class="glow-btn w-full">Save</button>`; break;
case 'plans': html=document.querySelector('section').innerHTML; break;
}
panel.innerHTML=html;
}
function closePanel(){document.getElementById('sidePanel').style.right='-420px';}

function buyPlan(amount,btn){
openPanel('deposit');
document.getElementById('depositAmount').innerText='Amount Selected: '+amount+' PKR';
for(let i=0;i<20;i++){
let conf=document.createElement('div');conf.className='confetti-piece';
conf.style.left=Math.random()*btn.offsetWidth+'px';
conf.style.background='hsl('+Math.random()*360+',100%,50%)';
conf.style.animationDuration=(Math.random()*1+0.5)+'s';
btn.appendChild(conf);setTimeout(()=>conf.remove(),1500);}
}

function copyText(text){navigator.clipboard.writeText(text);alert("Copied: "+text);}
</script>

<!-- LOGOUT -->
<button id="logoutBtn" onclick="logoutUser()">Logout</button>

<!-- DASHBOARD HIDDEN -->
<div id="dashboard" style="display:none;"></div>

</body>
</html>
