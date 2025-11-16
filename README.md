<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Neon Dashboard</title>

<!-- Tailwind -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"/>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: radial-gradient(circle at center,#0a0a0a,#111);
  color: #fff;
  overflow-x: hidden;
}

/* Neon Floating Particles */
.particles div {
  position: absolute;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: #00ffff;
  opacity: 0.3;
  animation: float 12s linear infinite;
}
@keyframes float {
  0% {transform: translateY(0);}
  100% {transform: translateY(-900px);}
}

/* Neon Logo Animation */
.logo-animate {
  font-size: 42px;
  font-weight: 900;
  background: linear-gradient(90deg,#00ffff,#ff00ff,#00ffff);
  -webkit-background-clip: text;
  color: transparent;
  animation: neonGlow 3s infinite alternate;
}
@keyframes neonGlow {
  0% {text-shadow: 0 0 5px #00ffff, 0 0 10px #ff00ff;}
  50% {text-shadow: 0 0 15px #00ffff,0 0 25px #ff00ff;}
  100% {text-shadow: 0 0 5px #00ffff,0 0 10px #ff00ff;}
}

/* Card Style */
.card {
  background: rgba(255,255,255,0.05);
  backdrop-filter: blur(15px);
  border-radius: 15px;
  padding: 20px;
  transition: transform 0.3s, box-shadow 0.3s;
}
.card:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px #00ffff, 0 0 30px #ff00ff;
}

button {
  background: linear-gradient(90deg,#00ffff,#ff00ff);
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  color: #000;
  font-weight: bold;
  transition: transform 0.2s, box-shadow 0.2s;
}
button:hover { transform: scale(1.05); box-shadow: 0 0 15px #00ffff,0 0 20px #ff00ff; }

input {
  width: 100%;
  padding: 10px;
  border-radius: 6px;
  border: none;
  margin-bottom: 10px;
  background: rgba(255,255,255,0.1);
  color: white;
}
</style>
</head>

<body>

<!-- Floating Particles -->
<div class="particles">
  <div style="left:5%; animation-duration:10s;"></div>
  <div style="left:25%; animation-duration:12s;"></div>
  <div style="left:45%; animation-duration:9s;"></div>
  <div style="left:65%; animation-duration:11s;"></div>
  <div style="left:85%; animation-duration:13s;"></div>
</div>

<div class="p-6">

  <!-- Logo -->
  <h1 class="text-center mb-6 logo-animate">ROCK EARN</h1>

  <!-- AUTH -->
  <div id="authBox" class="max-w-sm mx-auto card">
    <h2 id="authTitle" class="text-xl font-bold mb-3">Login</h2>
    <input id="authName" placeholder="Name (Signup only)">
    <input id="authEmail" placeholder="Email">
    <input id="authPass" placeholder="Password" type="password">
    <button onclick="authAction()">Continue</button>
    <p class="mt-3 text-center cursor-pointer" onclick="switchAuth()">Switch to Signup</p>
  </div>

  <!-- DASHBOARD -->
  <div id="dashboard" class="hidden">
    <h2 class="text-xl text-center mb-4">Welcome, <span id="uName"></span></h2>

    <div class="grid grid-cols-2 gap-4">
      <div class="card text-center cursor-pointer" onclick="openSection('deposit')">
        <i class="fa-solid fa-wallet text-3xl mb-2"></i>Deposit
      </div>
      <div class="card text-center cursor-pointer" onclick="openSection('withdraw')">
        <i class="fa-solid fa-money-bill text-3xl mb-2"></i>Withdraw
      </div>
      <div class="card text-center cursor-pointer" onclick="openSection('profile')">
        <i class="fa-solid fa-user text-3xl mb-2"></i>Profile
      </div>
      <div class="card text-center cursor-pointer" onclick="logout()">
        <i class="fa-solid fa-arrow-right-from-bracket text-3xl mb-2"></i>Logout
      </div>
    </div>

    <div id="sectionBox" class="card mt-6"></div>
  </div>

</div>

<script>
let users = JSON.parse(localStorage.getItem("users") || "[]");
let currentUser = JSON.parse(localStorage.getItem("currentUser") || "null");
let mode = "login";

/* Auto login */
if(currentUser) showDashboard();

function switchAuth(){
  mode = mode === "login" ? "signup" : "login";
  document.getElementById("authTitle").innerText = mode==="login"?"Login":"Signup";
  document.getElementById("authName").style.display = mode==="signup"?"block":"none";
}

function authAction(){
  let n=document.getElementById("authName").value.trim();
  let e=document.getElementById("authEmail").value.trim();
  let p=document.getElementById("authPass").value.trim();
  if(!e||!p) return alert("Enter email & password");

  if(mode==="signup"){
    if(!n) return alert("Enter name");
    if(users.find(u=>u.email===e)) return alert("Already registered");
    users.push({name:n,email:e,pass:p,balance:0});
    localStorage.setItem("users",JSON.stringify(users));
    alert("Signup successful, now login"); switchAuth(); return;
  }

  let u = users.find(u=>u.email===e && u.pass===p);
  if(!u) return alert("Invalid login");

  currentUser=u;
  localStorage.setItem("currentUser",JSON.stringify(u));
  showDashboard();
}

function showDashboard(){
  document.getElementById("authBox").style.display="none";
  document.getElementById("dashboard").classList.remove("hidden");
  document.getElementById("uName").innerText=currentUser.name;
}

function openSection(type){
  let box = document.getElementById("sectionBox");

  if(type==="deposit"){
    box.innerHTML = `<h3 class="text-lg mb-2">Deposit</h3>
      <p>JazzCash: <b>03705519562</b></p>
      <p>EasyPaisa: <b>03379827882</b></p>
      <input id="damt" placeholder="Amount">
      <button onclick="submitDeposit()">Submit</button>`;
  }
  if(type==="withdraw"){
    box.innerHTML = `<h3 class="text-lg mb-2">Withdraw</h3>
      <input id="wname" placeholder="Your Name">
      <input id="wacc" placeholder="Account Number">
      <input id="wamt" placeholder="Amount">
      <button onclick="submitWithdraw()">Submit</button>`;
  }
  if(type==="profile"){
    box.innerHTML = `<h3 class="text-lg mb-2">Profile</h3>
      <p>Name: ${currentUser.name}</p>
      <p>Email: ${currentUser.email}</p>
      <p>Balance: ${currentUser.balance}</p>`;
  }
}

function submitDeposit(){
  let amt=parseInt(document.getElementById("damt").value);
  if(!amt) return alert("Enter amount");
  currentUser.balance+=amt;
  save(); alert("Deposit added"); openSection('profile');
}
function submitWithdraw(){
  let amt=parseInt(document.getElementById("wamt").value);
  if(!amt) return alert("Enter amount");
  if(amt>currentUser.balance) return alert("Insufficient balance");
  currentUser.balance-=amt;
  save(); alert("Withdraw complete"); openSection('profile');
}
function save(){
  users = users.map(u=>u.email===currentUser.email?currentUser:u);
  localStorage.setItem("users",JSON.stringify(users));
  localStorage.setItem("currentUser",JSON.stringify(currentUser));
}
function logout(){localStorage.removeItem("currentUser");location.reload();}
</script>

</body>
</html>
