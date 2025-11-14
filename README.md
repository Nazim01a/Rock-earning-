<ROCK EARNING><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn - Full Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0f1123,#1c1f3b);color:white;margin:0;}
.btn{background:#5d5fef;padding:10px 18px;border-radius:10px;color:white;font-weight:600;cursor:pointer;display:inline-block;margin-top:5px;width:100%;}
.btn:hover{opacity:0.85;transform: scale(1.05);}
input, select{background:#1e213d;border:none;padding:10px;width:100%;border-radius:10px;color:white;margin-top:8px;}
.logo{font-size:28px;font-weight:bold;text-align:center;margin:20px 0;}
.card{background:#16182e;border-radius:14px;padding:18px;color:white;box-shadow:0 8px 20px rgba(0,0,0,0.5);margin-bottom:10px;}
.btn-deposit{background:#1dd11d;}
.btn-withdraw{background:#f5b700;color:black;}
.btn-logout{background:#ff4d4d;}
.scroll{max-height:300px;overflow-y:auto;}
</style>
</head>
<body><div class="logo">🚀 Rock Earn</div><!-- AUTH PAGE --><div id="authPage" class="p-5 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Sign Up</h2>
<input id="su_name" placeholder="Username">
<input id="su_email" placeholder="Email">
<input id="su_pass" type="password" placeholder="Password">
<button class="btn" onclick="signup()">Sign Up</button><hr class="my-4 border-gray-600">
<h2 class="text-2xl font-bold mb-4 text-center">Login</h2>
<input id="li_email" placeholder="Email">
<input id="li_pass" type="password" placeholder="Password">
<button class="btn" onclick="login()">Login</button>
</div><!-- DASHBOARD --><div id="dashboard" class="hidden p-5 max-w-5xl mx-auto">
<div class="card">
<p id="welcome"></p>
<p>Available Balance: <span id="balance">₨ 0</span></p>
<div class="flex gap-2 flex-wrap">
<button class="btn btn-deposit" onclick="showDeposit()">Deposit</button>
<button class="btn btn-withdraw" onclick="showWithdraw()">Withdraw</button>
<button class="btn" onclick="showPlans()">Plans</button>
<button class="btn" onclick="showShare()">Share Link</button>
<button class="btn" onclick="showProfile()">Profile</button>
<button class="btn btn-logout" onclick="logout()">Logout</button>
</div>
</div>
<div id="plansArea" class="scroll hidden"></div>
<div id="depositArea" class="hidden card"></div>
<div id="withdrawArea" class="hidden card"></div>
<div id="shareArea" class="hidden card"></div>
<div id="profileArea" class="hidden card"></div>
</div><script>
// ===== USER DATA =====
let users = JSON.parse(localStorage.getItem("rockUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("rockCurrentUser")) || null;
let balance = Number(localStorage.getItem("rockBalance")) || 0;

const plans = [
  {name:"Starter Plan", price:180, profit:20, days:20},
  {name:"Silver Plan", price:350, profit:40, days:25},
  {name:"Gold Plan", price:600, profit:70, days:30},
  {name:"Pro Plan", price:1200, profit:150, days:35},
  {name:"Advance Plan", price:2500, profit:320, days:40},
];

// ===== ON LOAD =====
window.onload = function(){
  if(currentUser){
    showDashboard();
  }
  updateBalance();
};

// ===== AUTH =====
function signup(){
  let n = document.getElementById("su_name").value.trim();
  let e = document.getElementById("su_email").value.trim();
  let p = document.getElementById("su_pass").value.trim();
  if(!n||!e||!p){alert("Fill all fields"); return;}
  if(users.find(u=>u.email===e)){alert("Email already registered"); return;}
  let u = {name:n,email:e,password:p};
  users.push(u);
  localStorage.setItem("rockUsers", JSON.stringify(users));
  currentUser = u;
  localStorage.setItem("rockCurrentUser", JSON.stringify(currentUser));
  localStorage.setItem("rockBalance", balance);
  showDashboard();
}

function login(){
  let e = document.getElementById("li_email").value.trim();
  let p = document.getElementById("li_pass").value.trim();
  if(!e||!p){alert("Fill all fields"); return;}
  let u = users.find(u=>u.email===e && u.password===p);
  if(!u){alert("Invalid credentials"); return;}
  currentUser = u;
  localStorage.setItem("rockCurrentUser", JSON.stringify(currentUser));
  showDashboard();
}

function logout(){
  currentUser = null;
  localStorage.removeItem("rockCurrentUser");
  document.getElementById("dashboard").style.display="none";
  document.getElementById("authPage").style.display="block";
}

function showDashboard(){
  document.getElementById("authPage").style.display="none";
  document.getElementById("dashboard").style.display="block";
  document.getElementById("welcome").innerText = "Welcome, "+currentUser.name;
  updateBalance();
  hideAllSections();
}

function updateBalance(){
  balance = Number(localStorage.getItem("rockBalance")) || 0;
  document.getElementById("balance").innerText = "₨ "+balance;
}

// ===== NAVIGATION =====
function hideAllSections(){
  document.getElementById("plansArea").classList.add("hidden");
  document.getElementById("depositArea").classList.add("hidden");
  document.getElementById("withdrawArea").classList.add("hidden");
  document.getElementById("shareArea").classList.add("hidden");
  document.getElementById("profileArea").classList.add("hidden");
}

// ===== PLANS =====
function showPlans(){
  hideAllSections();
  let html = '';
  plans.forEach((p,i)=>{
    html += `<div class='card'><h3>${p.name}</h3><p>Price: ${p.price} PKR</p><p>Daily Profit: ${p.profit}</p><p>Days: ${p.days}</p><button class='btn btn-deposit mt-2' onclick='showDeposit(${p.price})'>Buy Now</button></div>`;
  });
  let area = document.getElementById("plansArea");
  area.innerHTML = html;
  area.classList.remove("hidden");
}

// ===== DEPOSIT =====
function showDeposit(amount=0){
  hideAllSections();
  let area = document.getElementById("depositArea");
  area.innerHTML = `<h3>Deposit</h3>
<select id='depMethod'><option value='jazz'>JazzCash</option><option value='easy'>EasyPaisa</option></select>
<p>JazzCash: 03705519562 | EasyPaisa: 03379827882</p>
<input id='depAmt' type='number' placeholder='Enter Amount' value='${amount}'>
<input id='trxId' placeholder='Transaction ID'>
<input id='proof' type='file'>
<button class='btn btn-deposit mt-2' onclick='submitDeposit()'>Submit Deposit</button>`;
  area.classList.remove("hidden");
}

function submitDeposit(){
  let amt = Number(document.getElementById('depAmt').value);
  if(!amt || amt<=0){alert('Enter valid amount'); return;}
  balance += amt;
  localStorage.setItem("rockBalance", balance);
  updateBalance();
  alert('Deposit successful');
  hideAllSections();
}

// ===== WITHDRAW =====
function showWithdraw(){
  hideAllSections();
  let area = document.getElementById("withdrawArea");
  area.innerHTML = `<h3>Withdraw</h3>
<select id='withMethod'><option value='jazz'>JazzCash</option><option value='easy'>EasyPaisa</option><option value='bank'>Bank</option></select>
<input id='wName' placeholder='Full Name'>
<input id='wAcc' placeholder='Account / Number'>
<input id='wAmt' type='number' placeholder='Enter Amount'>
<button class='btn btn-withdraw mt-2' onclick='submitWithdraw()'>Withdraw</button>`;
  area.classList.remove("hidden");
}

function submitWithdraw(){
  let amt = Number(document.getElementById('wAmt').value);
  if(!amt || amt<=0){alert('Enter valid amount'); return;}
  if(amt>balance){alert('Insufficient balance'); return;}
  balance -= amt;
  localStorage.setItem("rockBalance", balance);
  updateBalance();
  alert('Withdrawal requested');
  hideAllSections();
}

// ===== SHARE =====
function showShare(){
  hideAllSections();
  let area = document.getElementById('shareArea');
  area.innerHTML = `<h3>Share Your Link</h3><input id='shareLink' value='https://rockearn.com?ref=ROCK123'><button class='btn mt-2' onclick='copyLink()'>Copy Link</button>`;
  area.classList.remove('hidden');
}

function copyLink(){
  let link = document.getElementById('shareLink');
  link.select();
  document.execCommand('copy');
  alert('Link Copied!');
}

// ===== PROFILE =====
function showProfile(){
  hideAllSections();
  let area = document.getElementById('profileArea');
  area.innerHTML = `<h3>Profile</h3><p>Username: ${currentUser.name}</p><p>Email: ${currentUser.email}</p>
<h4>Company Details</h4>
<p>Rock Earn Inc, Founded: 2018, Based in California, USA</p>
<p>Working with crypto & Binance</p>`;
  area.classList.remove('hidden');
}
</script></body>
</html>
