<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Pro — Premium Animated</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body {
  font-family:'Segoe UI',sans-serif;
  background: linear-gradient(135deg, #0b0f1a, #0f172a);
  color: white;
  margin:0;
  overflow-x:hidden;
}
header h1 {
  text-shadow: 0 0 15px #00ffe0, 0 0 25px #5d5fef;
}
.neon-card {
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.1);
  backdrop-filter: blur(15px);
  border-radius: 20px;
  padding: 20px;
  transition: all 0.3s ease;
}
.neon-card:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px #00ffe0, 0 0 40px #5d5fef;
}
button {
  transition: all 0.3s ease;
}
button:hover {
  transform: scale(1.05);
}
#sidePanel {
  position: fixed;
  top: 0;
  right: -400px;
  width: 380px;
  height: 100vh;
  background: #0f172a;
  padding: 20px;
  overflow-y:auto;
  transition: all 0.4s ease;
  z-index:999;
  border-left: 2px solid #00ffe0;
}
#closePanel {
  position:absolute;
  top:10px;
  right:10px;
  cursor:pointer;
  background:red;
  color:white;
  padding:5px 10px;
  border-radius:5px;
}
.fixed-top {
  position:sticky;
  top:0;
  background: rgba(0,0,0,0.3);
  backdrop-filter: blur(10px);
  padding:10px 20px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  z-index:100;
  border-bottom: 1px solid #00ffe0;
}
.glow-btn {
  background: linear-gradient(90deg,#5d5fef,#00ffe0);
  color:#fff;
  padding:10px 15px;
  border-radius:12px;
  font-weight:600;
  cursor:pointer;
  box-shadow: 0 0 10px #5d5fef, 0 0 20px #00ffe0;
}
.glow-btn:hover {
  box-shadow: 0 0 20px #5d5fef, 0 0 40px #00ffe0;
  transform: scale(1.05);
}
#logoutBtn {
  position: fixed;
  bottom: 20px;
  left:50%;
  transform: translateX(-50%);
  background: #ff4d4d;
  padding: 12px 20px;
  border-radius: 12px;
  cursor:pointer;
  font-weight:600;
  box-shadow:0 0 15px #ff4d4d;
  transition:all 0.3s ease;
}
#logoutBtn:hover {
  transform:translateX(-50%) scale(1.05);
  box-shadow:0 0 25px #ff4d4d;
}
</style>
</head>
<body>

<!-- AUTH SYSTEM -->
<div id="authBox" class="p-6 max-w-md mx-auto mt-10 bg-white/5 rounded-2xl shadow-lg">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2" />
<input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2" />
<input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2" />
<button onclick="signupUser()" class="w-full mb-2 glow-btn">Sign Up</button>
<button onclick="loginUser()" class="w-full glow-btn">Login</button>
</div>

<script>
let users = JSON.parse(localStorage.getItem("reUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("reCurrent")) || null;

function signupUser(){
  let n = document.getElementById('authName').value.trim();
  let e = document.getElementById('authEmail').value.trim();
  let p = document.getElementById('authPass').value.trim();
  if(!n||!e||!p){ alert('Fill all fields'); return; }
  if(users.find(u=>u.email===e)){ alert('Email already registered'); return; }
  let u = {name:n,email:e,pass:p};
  users.push(u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(u));
  loadDashboard();
}
function loginUser(){
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  let u=users.find(u=>u.email===e && u.pass===p);
  if(!u){ alert('Invalid credentials'); return; }
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(u));
  loadDashboard();
}
function loadDashboard(){
  document.getElementById('authBox').style.display='none';
}
function logoutUser(){
  currentUser=null;
  localStorage.removeItem('reCurrent');
  location.reload();
}
</script>

<!-- TOP INFO BAR -->
<div class="fixed-top">
  <div id="userInfo">Welcome, ${currentUser ? currentUser.name : 'User'}</div>
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
</div>

<!-- PLANS -->
<section class="mt-10 text-center">
<h2 class="text-3xl font-bold mb-6">Our Premium Plans</h2>
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 px-4">
  <div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Starter Plan</h3><p>Amount: 180 PKR</p><p>Daily Profit: 20 PKR</p><button onclick="buyPlan(180)" class="glow-btn mt-3">Buy Now</button></div>
  <div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Basic Plan</h3><p>Amount: 500 PKR</p><p>Daily Profit: 60 PKR</p><button onclick="buyPlan(500)" class="glow-btn mt-3">Buy Now</button></div>
  <div class="neon-card p-6"><h3 class="text-xl font-semibold mb-1">Standard Plan</h3><p>Amount: 1000 PKR</p><p>Daily Profit: 130 PKR</p><button onclick="buyPlan(1000)" class="glow-btn mt-3">Buy Now</button></div>
</div>
</section>

<!-- SIDE PANEL -->
<div id="sidePanel"><div id="closePanel" onclick="closePanel()">X</div></div>

<script>
function openPanel(type){
  let panel=document.getElementById('sidePanel'); panel.style.right='0';
  if(type==='profile'){panel.innerHTML=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Profile</h2><p>Name: ${currentUser ? currentUser.name:'User'}</p><p>ID: RE-928373</p><button onclick="copyText('https://rockearnpro.com/user/928373')" class="glow-btn mt-2">Copy Link</button>`;}
  if(type==='deposit'){panel.innerHTML=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Deposit</h2><p id='depositAmount'>Amount Selected: 0 PKR</p><p>JazzCash: 03705519562 <button onclick="copyText('03705519562')" class="glow-btn mt-1">Copy</button></p><p>EasyPaisa: 03379827882 <button onclick="copyText('03379827882')" class="glow-btn mt-1">Copy</button></p><input type='file' class='mt-3'><input type='text' placeholder='Transaction ID' class='mt-3 p-2 w-full rounded bg-white/20'><button class="glow-btn mt-4 w-full">Submit</button>`;}
  if(type==='withdraw'){panel.innerHTML=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Withdraw</h2><input type='number' placeholder='Amount' class='w-full p-2 rounded bg-white/20 mb-3'><select class='w-full p-2 rounded bg-white/20 mb-3'><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number' class='w-full p-2 rounded bg-white/20 mb-3'><button class="glow-btn w-full">Withdraw</button>`;}
  if(type==='activity'){panel.innerHTML=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Activity History</h2><p>• Deposit: 500 PKR</p><p>• Withdraw: 300 PKR</p><p>• Plan Bought: Basic Plan</p>`;}
  if(type==='company'){panel.innerHTML=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Company</h2><p>Since: <b>2018</b></p><p>Industry: <b>Crypto & FinTech</b></p><p>Partner: <b>Binance</b></p><p>Address: 1234 Crypto Avenue, San Francisco, California, USA</p><p>Email: support@rockearnpro.com</p>`;}
  if(type==='settings'){panel.innerHTML=`<div id="closePanel" onclick="closePanel()">X</div><h2 class='text-2xl font-bold mb-4'>Settings</h2><input type='text' placeholder='Change Name' class='w-full p-2 rounded bg-white/20 mb-3'><input type='password' placeholder='Change Password' class='w-full p-2 rounded bg-white/20 mb-3'><button class="glow-btn w-full">Save</button>`;}
}
function closePanel(){document.getElementById('sidePanel').style.right='-400px';}
function buyPlan(amount){
  openPanel('deposit');
  document.getElementById('depositAmount').innerText='Amount Selected: '+amount+' PKR';
}
function copyText(text){navigator.clipboard.writeText(text);alert("Copied: "+text);}
</script>

<!-- LOGOUT -->
<button id="logoutBtn" onclick="logoutUser()">Logout</button>

</body>
</html>
