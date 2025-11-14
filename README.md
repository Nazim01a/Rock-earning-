<ROCK ONE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Official</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body { font-family:'Segoe UI',sans-serif; background:linear-gradient(120deg,#0f1123,#1c1f3b); color:white; margin:0; }
.card { background:#16182e; border-radius:14px; padding:18px; color:white; box-shadow:0 8px 20px rgba(0,0,0,0.5); transition: transform 0.2s;}
.card:hover { transform: scale(1.02); }
.btn { background:#5d5fef; padding:10px 18px; border-radius:10px; color:white; font-weight:600; cursor:pointer; text-align:center; display:inline-block; transition: transform 0.2s, opacity 0.2s; margin-top:5px; }
.btn:hover { opacity:0.85; transform: scale(1.05); }
input, select { background:#1e213d; border:none; padding:10px; width:100%; border-radius:10px; color:white; margin-top:8px; }
h2,h3 { margin:0; }
.logo { font-size:28px; font-weight:bold; background: linear-gradient(90deg,#5d5fef,#00ffe0); -webkit-background-clip:text; -webkit-text-fill-color:transparent; animation: glow 2s infinite alternate;}
@keyframes glow {0% { filter: drop-shadow(0 0 5px #5d5fef);} 100% { filter: drop-shadow(0 0 20px #00ffe0);} }
.scroll { max-height:300px; overflow-y:auto; }
.btn-deposit { background:#1dd11d; }
.btn-withdraw { background:#f5b700; color:black; }
.btn-back { background:#555; }
</style>
</head>
<body>

<!-- HEADER -->
<div id="header" class="p-5 flex justify-between items-center bg-[#0f1123]">
  <div class="logo">🚀 Rock Earn</div>
  <div id="headerUser" class="text-white font-semibold"></div>
</div>

<!-- AUTH PAGE -->
<div id="authPage" class="p-5 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Sign Up / Login</h2>

<h3 class="text-xl font-semibold mt-4">Sign Up</h3>
<input id="su_name" placeholder="Username">
<input id="su_email" placeholder="Email">
<input id="su_pass" type="password" placeholder="Password">
<button class="btn w-full" onclick="signup()">Sign Up</button>

<h3 class="text-xl font-semibold mt-4">Login</h3>
<input id="li_email" placeholder="Email">
<input id="li_pass" type="password" placeholder="Password">
<button class="btn w-full" onclick="login()">Login</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="hidden p-5 max-w-5xl mx-auto">
<div class="card mb-4 flex justify-between items-center">
  <div>
    <div class="text-gray-400">Available Balance</div>
    <div id="balance" class="text-2xl font-bold">₨ 0</div>
  </div>
  <div class="flex gap-2 flex-wrap">
    <button class="btn" onclick="showDeposit()">Deposit</button>
    <button class="btn" onclick="showWithdraw()">Withdraw</button>
    <button class="btn" onclick="showShare()">Share Link</button>
    <button class="btn" onclick="showProfile()">Profile</button>
    <button class="btn" onclick="showSupport()">Support</button>
    <button class="btn bg-red-600" onclick="logout()">Logout</button>
  </div>
</div>
<h3 class="text-xl font-bold mb-2">Investment Plans</h3>
<div id="plansArea" class="grid md:grid-cols-2 gap-4 scroll"></div>
</div>

<!-- PAGES -->
<div id="planPage" class="hidden p-5 max-w-3xl mx-auto card"></div>
<div id="depositPage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-2xl font-bold mb-3">Deposit</h2>
<label>Choose Method:</label>
<select id="depositMethod">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
<option value="bank">Bank Account</option>
</select>
<div class="mt-2">
<p><b>JazzCash Number:</b> 03705519562</p>
<p><b>EasyPaisa Number:</b> 03379827882</p>
<p><b>Bank Account:</b> 1234567890 | Bank XYZ</p>
</div>
<input id="depositAmount" placeholder="Enter Amount">
<input id="trxid" placeholder="Transaction ID">
<input id="proof" type="file">
<button class="btn btn-deposit w-full mt-4" onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawPage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-2xl font-bold mb-3">Withdraw</h2>
<select id="withdrawMethod">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
<option value="bank">Bank Account</option>
</select>
<input id="wNumber" placeholder="Your Number / Account">
<input id="wAmount" placeholder="Enter Amount">
<button class="btn btn-withdraw w-full mt-4" onclick="submitWithdraw()">Submit Withdrawal</button>
</div>

<div id="sharePage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-xl font-bold mb-3">Share Your Link</h2>
<input id="shareLink" value="https://rockearn.com?ref=ROCK123">
<button class="btn mt-3 w-full" onclick="copyLink()">Copy Link</button>
</div>

<div id="profilePage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-xl font-bold mb-3">Your Profile</h2>
<p id="pUser"></p>
<p id="pMail"></p>
</div>

<div id="supportPage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-2xl font-bold mb-3">Customer Service</h2>
<p>WhatsApp: <b>0318-7102225</b></p>
<h2 class="text-xl font-bold mt-4 mb-2">Company Details</h2>
<p>Company: <b>Rock Earn Digital Pvt Ltd</b></p>
<p>Launched: <b>2024</b></p>
<p>Service Hours: <b>24/7</b></p>
<p>Head Office: <b>Lahore, Pakistan</b></p>
</div>

<script>
// ===== USERS DATA =====
let users = JSON.parse(localStorage.getItem("rockUsers")) || [];
let currentUser = null;
let balance = 0;

// ===== PLANS =====
const plans = [
 {name:"Starter Plan", price:180, profit:20, days:20},
 {name:"Silver Plan", price:350, profit:40, days:25},
 {name:"Gold Plan", price:600, profit:70, days:30},
 {name:"Pro Plan", price:1200, profit:150, days:35},
 {name:"Advance Plan", price:2500, profit:320, days:40},
 {name:"Ultra Plan", price:4000, profit:520, days:45},
 {name:"Premium Plan", price:7000, profit:900, days:50},
 {name:"Coming Soon 1", price:0, profit:0, days:0},
 {name:"Coming Soon 2", price:0, profit:0, days:0}
];

// ===== ON LOAD =====
window.onload = function(){
  if(localStorage.getItem("rockCurrentUser")){
    currentUser = JSON.parse(localStorage.getItem("rockCurrentUser"));
    balance = Number(localStorage.getItem("rockBalance")) || 0;
    showDashboard();
  }
  loadPlans();
}

// ===== SIGNUP =====
function signup(){
  let uname = document.getElementById("su_name").value.trim();
  let email = document.getElementById("su_email").value.trim();
  let pass = document.getElementById("su_pass").value.trim();
  if(!uname || !email || !pass){ alert("Fill all fields"); return; }

  let exist = users.find(u=>u.email===email);
  if(exist){ alert("Email already registered. Use login."); return; }

  let newUser = {name: uname,email: email,password: pass};
  users.push(newUser);
  localStorage.setItem("rockUsers", JSON.stringify(users));
  localStorage.setItem("rockCurrentUser", JSON.stringify(newUser));
  localStorage.setItem("rockBalance", 0);
  currentUser = newUser;
  balance = 0;
  alert("Sign Up Successful!");
  showDashboard();
}

// ===== LOGIN =====
function login(){
  let email = document.getElementById("li_email").value.trim();
  let pass = document.getElementById("li_pass").value.trim();
  if(!email || !pass){ alert("Fill all fields"); return; }

  let exist = users.find(u=>u.email===email && u.password===pass);
  if(!exist){ alert("Invalid credentials"); return; }

  currentUser = exist;
  localStorage.setItem("rockCurrentUser", JSON.stringify(currentUser));
  balance = Number(localStorage.getItem("rockBalance")) || 0;
  alert("Login Successful!");
  showDashboard();
}

// ===== DASHBOARD =====
function showDashboard(){
  document.getElementById("authPage").style.display="none";
  document.getElementById("dashboard").style.display="block";
  document.getElementById("headerUser").innerText = "Welcome, "+currentUser.name;
  document.getElementById("pUser")?.innerText = "Username: "+currentUser.name;
  document.getElementById("pMail")?.innerText = "Email: "+currentUser.email;
  updateBalance();
}

// ===== LOGOUT =====
function logout(){
  localStorage.removeItem("rockCurrentUser");
  localStorage.setItem("rockBalance", balance);
  location.reload();
}

// ===== BALANCE =====
function updateBalance(){
  document.getElementById("balance").innerText = "₨ "+balance;
}

// ===== LOAD PLANS =====
function loadPlans(){
  let html="";
  plans.forEach((p,i)=>{
    let isComing = p.price===0;
    html += `<div class='card'>
      <h3 class='text-xl font-bold mb-1'>${p.name}</h3>
      ${isComing?`<p class="text-yellow-400 font-semibold">Coming Soon</p>`:`
      <p>Price: <b>${p.price} PKR</b></p>
      <p>Daily Profit: <b>${p.profit} Rs/day</b></p>
      <p>Duration: <b>${p.days} Days</b></p>
      <p>Total Return: <b>${p.profit*p.days} Rs</b></p>`}
      <button class='btn mt-2 w-full' onclick='openPlan(${i})'>View Details</button>
    </div>`;
  });
  document.getElementById("plansArea").innerHTML = html;
}

// ===== OPEN PLAN =====
function openPlan(i){
  let p = plans[i];
  hideAll();
  document.getElementById("planPage").style.display="block";
  if(p.price===0){
    document.getElementById("planPage").innerHTML = `<h2 class='text-2xl font-bold mb-2'>${p.name}</h2>
    <p class="text-yellow-400 font-semibold">This plan is coming soon!</p>
    <button class='btn btn-back w-full mt-4' onclick='backHome()'>Back</button>`;
    return;
  }
  document.getElementById("planPage").innerHTML = `<h2 class='text-2xl font-bold mb-2'>${p.name}</h2>
  <p>Price: ${p.price} PKR</p>
  <p>Daily Profit: ${p.profit} Rs</p>
  <p>Duration: ${p.days} Days</p>
  <p>Total Profit: ${p.profit*p.days} Rs</p>
  <input id="planDepositAmount" type="number" placeholder="Amount (Default: ${p.price})" value="${p.price}">
  <button class='btn btn-deposit w-full mt-4' onclick='submitDeposit(true)'>Deposit</button>
  <button class='btn btn-withdraw w-full mt-2' onclick='showWithdraw()'>Withdraw</button>
  <button class='btn btn-back w-full mt-2' onclick='backHome()'>Back</button>`;
}

// ===== NAVIGATION =====
function showDeposit(){ hideAll(); document.getElementById("depositPage").style.display="block"; }
function showWithdraw(){ hideAll(); document.getElementById("withdrawPage").style.display="block"; }
function showShare(){ hideAll(); document.getElementById("sharePage").style.display="block"; }
function showProfile(){ hideAll(); document.getElementById("profilePage").style.display="block"; }
function showSupport(){ hideAll(); document.getElementById("supportPage").style.display="block"; }
function backHome(){ hideAll(); document.getElementById("dashboard").style.display="block"; }
function hideAll(){ document.querySelectorAll("body > div").forEach(d=>d.style.display="none"); }

// ===== DEPOSIT =====
function submitDeposit(fromPlan=false){
  let amt;
  if(fromPlan) amt = Number(document.getElementById("planDepositAmount").value);
  else amt = Number(document.getElementById("depositAmount").value);
  if(!amt || amt<=0){ alert("Enter valid amount"); return; }
  balance += amt;
  updateBalance();
  localStorage.setItem("rockBalance", balance);
  alert("Deposit successful! Balance updated.");
  backHome();
}

// ===== WITHDRAW =====
function submitWithdraw(){
  let amt = Number(document.getElementById("wAmount").value);
  if(!amt || amt<=0){ alert("Enter valid amount"); return; }
  if(amt>balance){ alert("Insufficient balance"); return; }
  balance -= amt;
  updateBalance();
  localStorage.setItem("rockBalance", balance);
  alert("Withdrawal requested successfully!");
  backHome();
}

// ===== COPY LINK =====
function copyLink(){
  let link = document.getElementById("shareLink");
  link.select();
  document.execCommand("copy");
  alert("Link Copied!");
}
</script>

</body>
</html>
