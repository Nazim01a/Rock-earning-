<WELCOME TO ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Official</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0f1123,#1c1f3b);color:white;margin:0;}
.card{background:#16182e;border-radius:14px;padding:18px;color:white;box-shadow:0 8px 20px rgba(0,0,0,0.5);transition: transform 0.2s;}
.card:hover{transform: scale(1.02);}
.btn{background:#5d5fef;padding:10px 18px;border-radius:10px;color:white;font-weight:600;cursor:pointer;text-align:center;display:inline-block;transition: transform 0.2s, opacity 0.2s;margin-top:5px;}
.btn:hover{opacity:0.85;transform: scale(1.05);}
input,select{background:#1e213d;border:none;padding:10px;width:100%;border-radius:10px;color:white;margin-top:8px;}
h2,h3{margin:0;}
.logo{font-size:28px;font-weight:bold;background: linear-gradient(90deg,#5d5fef,#00ffe0);-webkit-background-clip:text;-webkit-text-fill-color:transparent;animation: glow 2s infinite alternate;}
@keyframes glow{0%{filter: drop-shadow(0 0 5px #5d5fef);}100%{filter: drop-shadow(0 0 20px #00ffe0);}}
.scroll{max-height:300px;overflow-y:auto;}
.btn-deposit{background:#1dd11d;}
.btn-withdraw{background:#f5b700;color:black;}
.btn-back{background:#555;}
</style>
</head>
<body><div id="header" class="p-5 flex justify-between items-center bg-[#0f1123]">
  <div class="logo">🚀 Rock Earn</div>
  <div id="headerUser" class="text-white font-semibold"></div>
</div><!-- AUTH PAGE --><div id="authPage" class="p-5 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Sign Up / Login</h2><h3 class="text-xl font-semibold mt-4">Sign Up</h3>
<input id="su_name" placeholder="Username">
<input id="su_email" placeholder="Email">
<input id="su_pass" type="password" placeholder="Password">
<button class="btn w-full" onclick="signup()">Sign Up</button><h3 class="text-xl font-semibold mt-4">Login</h3>
<input id="li_email" placeholder="Email">
<input id="li_pass" type="password" placeholder="Password">
<button class="btn w-full" onclick="login()">Login</button>
</div><!-- DASHBOARD --><div id="dashboard" class="hidden p-5 max-w-5xl mx-auto">
<div class="card mb-4 flex justify-between items-center">
  <div>
    <div class="text-gray-400">Available Balance</div>
    <div id="balance" class="text-2xl font-bold">₨ 0</div>
  </div>
  <div class="flex gap-2 flex-wrap">
    <button class="btn" onclick="showDeposit()">Deposit</button>
    <button class="btn" onclick="showWithdraw()">Withdraw</button>
    <button class="btn" onclick="showProfile()">Profile</button>
    <button class="btn" onclick="showShare()">Share Link</button>
    <button class="btn bg-red-600" onclick="logout()">Logout</button>
  </div>
</div>
<h3 class="text-xl font-bold mb-2">Investment Plans</h3>
<div id="plansArea" class="grid md:grid-cols-2 gap-4 scroll"></div>
</div><!-- DEPOSIT PAGE --><div id="depositPage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-2xl font-bold mb-3">Deposit</h2>
<label>Enter Amount:</label>
<input id="depositAmount" placeholder="Enter Amount">
<label>Transaction ID:</label>
<input id="trxid" placeholder="Enter Transaction ID">
<label>Upload Proof:</label>
<input id="proof" type="file">
<button class="btn btn-deposit w-full mt-4" onclick="submitDeposit()">Deposit</button>
<button class="btn btn-back w-full mt-2" onclick="backHome()">Back</button>
</div><!-- WITHDRAW PAGE --><div id="withdrawPage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-2xl font-bold mb-3">Withdraw</h2>
<input id="wAmount" placeholder="Enter Amount">
<button class="btn btn-withdraw w-full mt-4" onclick="submitWithdraw()">Withdraw</button>
<button class="btn btn-back w-full mt-2" onclick="backHome()">Back</button>
</div><!-- PROFILE PAGE --><div id="profilePage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-xl font-bold mb-3">Profile</h2>
<p id="pUser"></p>
<p id="pMail"></p>
<p id="pBalance"></p>
<button class="btn btn-back w-full mt-2" onclick="backHome()">Back</button>
</div><!-- SHARE LINK PAGE --><div id="sharePage" class="hidden p-5 max-w-md mx-auto card">
<h2 class="text-xl font-bold mb-3">Share Your Link</h2>
<input id="shareLink" value="https://rockearn.com?ref=ROCK123">
<button class="btn w-full mt-2" onclick="copyLink()">Copy Link</button>
<button class="btn btn-back w-full mt-2" onclick="backHome()">Back</button>
</div><script>
let users = JSON.parse(localStorage.getItem("rockUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("rockCurrentUser")) || null;
const plans=[{name:"Starter Plan",price:180,profit:20,days:20},{name:"Silver Plan",price:350,profit:40,days:25},{name:"Gold Plan",price:600,profit:70,days:30},{name:"Pro Plan",price:1200,profit:150,days:35},{name:"Advance Plan",price:2500,profit:320,days:40},{name:"Ultra Plan",price:4000,profit:520,days:45},{name:"Premium Plan",price:7000,profit:900,days:50}];

window.onload=function(){if(currentUser){showDashboard();}};

function signup(){
let n=document.getElementById("su_name").value.trim();
let e=document.getElementById("su_email").value.trim();
let p=document.getElementById("su_pass").value.trim();
if(!n||!e||!p){alert("Fill all fields");return;}
if(users.find(u=>u.email===e)){alert("Email already registered");return;}
let u={name:n,email:e,password:p,balance:0};
users.push(u);localStorage.setItem("rockUsers",JSON.stringify(users));currentUser=u;localStorage.setItem("rockCurrentUser",JSON.stringify(currentUser));alert("Signup successful!");showDashboard();}

function login(){
let e=document.getElementById("li_email").value.trim();
let p=document.getElementById("li_pass").value.trim();
if(!e||!p){alert("Fill all fields");return;}
let u=users.find(u=>u.email===e && u.password===p);
if(!u){alert("Invalid credentials");return;}
currentUser=u;localStorage.setItem("rockCurrentUser",JSON.stringify(currentUser));alert("Login successful!");showDashboard();}

function showDashboard(){document.getElementById("authPage").style.display="none";document.getElementById("dashboard").style.display="block";document.getElementById("headerUser").innerText="Welcome, "+currentUser.name;loadPlans();updateBalance();}

function loadPlans(){let html="";plans.forEach((p,i)=>{html+=`<div class='card'><h3 class='text-xl font-bold mb-1'>${p.name}</h3><p>Price: ${p.price}</p><p>Daily Profit: ${p.profit}</p><p>Days: ${p.days}</p><button class='btn w-full mt-2' onclick='planDeposit(${i})'>Deposit</button></div>`;});document.getElementById("plansArea").innerHTML=html;}

function planDeposit(index){let p=plans[index];currentUser.balance+=p.price;saveUser();updateBalance();alert(p.name+" deposited! Balance updated.");}

function updateBalance(){document.getElementById("balance").innerText="₨ "+currentUser.balance;document.getElementById("pBalance")?.innerText="Balance: ₨ "+currentUser.balance;}

function submitDeposit(){let amt=Number(document.getElementById("depositAmount").value);if(!amt||amt<=0){alert("Enter valid amount");return;}let trx=document.getElementById("trxid").value.trim();let proof=document.getElementById("proof").files[0];if(!trx||!proof){alert("Enter transaction ID and upload proof");return;}currentUser.balance+=amt;saveUser();updateBalance();alert("Deposit successful! Balance updated.");backHome();}

function submitWithdraw(){let amt=Number(document.getElementById("wAmount").value);if(!amt||amt<=0){alert("Enter valid amount");return;}if(amt>currentUser.balance){alert("Insufficient balance");return;}currentUser.balance-=amt;saveUser();updateBalance();alert("Withdraw successful!");backHome();}

function showProfile(){hideAll();document.getElementById("profilePage").style.display="block";document.getElementById("pUser").innerText="Username: "+currentUser.name;document.getElementById("pMail").innerText="Email: "+currentUser.email;updateBalance();}

function showShare(){hideAll();document.getElementById("sharePage").style.display="block";}
function copyLink(){let link=document.getElementById("shareLink");link.select();document.execCommand("copy");alert("Link copied!");}

function backHome(){hideAll();document.getElementById("dashboard").style.display="block";}
function showDeposit(){hideAll();document.getElementById("depositPage").style.display="block";}
function showWithdraw(){hideAll();document.getElementById("withdrawPage").style.display="block";}
function hideAll(){["dashboard","depositPage","withdrawPage","profilePage","sharePage"].forEach(id=>document.getElementById(id).style.display="none");}

function logout(){currentUser=null;localStorage.removeItem("rockCurrentUser");document.getElementById("dashboard").style.display="none";document.getElementById("authPage").style.display="block";}

function saveUser(){users=users.map(u=>u.email===currentUser.email?currentUser:u);localStorage.setItem("rockUsers",JSON.stringify(users));localStorage.setItem("rockCurrentUser",JSON.stringify(currentUser));}
</script></body>
</html>
