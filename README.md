<WELCOME TO ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn - Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0f1123,#1c1f3b);color:white;margin:0;}
.logo{font-size:28px;font-weight:bold;text-align:center;margin:20px 0;background: linear-gradient(90deg,#5d5fef,#00ffe0); -webkit-background-clip: text; -webkit-text-fill-color: transparent;}
.card{background:#16182e;border-radius:16px;padding:20px;color:white;box-shadow:0 10px 25px rgba(0,0,0,0.5);margin-bottom:15px;transition: transform 0.2s;}
.card:hover{transform: translateY(-3px);}
.btn{padding:12px 20px;border-radius:12px;color:white;font-weight:600;cursor:pointer;display:inline-block;margin-top:8px;width:100%;text-align:center;transition:all 0.3s;}
.btn:hover{opacity:0.85;transform: scale(1.05);}
.btn-deposit{background:#1dd11d;}
.btn-withdraw{background:#f5b700;color:black;}
.btn-logout{background:#ff4d4d;}
.btn-section{margin-top:15px;}
.btn-section div{margin-bottom:10px;}
.scroll{max-height:300px;overflow-y:auto;}
input, select{background:#1e213d;border:none;padding:10px;width:100%;border-radius:10px;color:white;margin-top:8px;}
</style>
</head>
<body>
<div class="logo">🚀 Rock Earn</div>
<div id="authPage" class="p-5 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Sign Up</h2>
<input id="su_name" placeholder="Username">
<input id="su_email" placeholder="Email">
<input id="su_pass" type="password" placeholder="Password">
<button class="btn" onclick="signup()">Sign Up</button>
<hr class="my-4 border-gray-600">
<h2 class="text-2xl font-bold mb-4 text-center">Login</h2>
<input id="li_email" placeholder="Email">
<input id="li_pass" type="password" placeholder="Password">
<button class="btn" onclick="login()">Login</button>
</div><div id="dashboard" class="hidden p-5 max-w-5xl mx-auto">
<div class="card">
<p id="welcome" class="text-xl font-semibold mb-2"></p>
<p>Available Balance: <span id="balance">₨ 0</span></p>
<div class="btn-section">
<div class="section-btn" onclick="showDeposit()">💰 Deposit</div>
<div class="section-btn" onclick="showWithdraw()">🏧 Withdraw</div>
<div class="section-btn" onclick="showPlans()">📈 Plans</div>
<div class="section-btn" onclick="showShare()">🔗 Share Link</div>
<div class="section-btn" onclick="showProfile()">👤 Profile</div>
<div class="section-btn" onclick="logout()">🚪 Logout</div>
</div>
</div><div id="plansArea" class="scroll hidden"></div>
<div id="depositArea" class="hidden card"></div>
<div id="withdrawArea" class="hidden card"></div>
<div id="shareArea" class="hidden card"></div>
<div id="profileArea" class="hidden card"></div><script>
let users = JSON.parse(localStorage.getItem("rockUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("rockCurrentUser")) || null;
let balance = Number(localStorage.getItem("rockBalance")) || 0;

const plans = [
  {name:"Starter Plan", price:180, profit:30, days:20},
  {name:"Bronze Plan", price:500, profit:90, days:25},
  {name:"Silver Plan", price:1000, profit:200, days:30},
  {name:"Gold Plan", price:2000, profit:420, days:35},
  {name:"Platinum Plan", price:4000, profit:880, days:40},
  {name:"Diamond Plan", price:7000, profit:1500, days:45},
  {name:"Elite Plan", price:10000, profit:2200, days:50},
  {name:"Titan Plan", price:15000, profit:3500, days:55},
  {name:"Master Plan", price:20000, profit:4800, days:60},
  {name:"Infinity Plan", price:25000, profit:6200, days:60},
  {name:"Ultra Plan", price:30000, profit:7800, days:65},
  {name:"Super Plan", price:35000, profit:9500, days:65},
  {name:"Mega Plan", price:40000, profit:11500, days:70},
  {name:"Coming Soon 1", price:0, profit:0, days:0},
  {name:"Coming Soon 2", price:0, profit:0, days:0}
];

window.onload = function(){
  if(currentUser){ showDashboard(); }
  updateBalance();
};

function signup(){
  let n=document.getElementById("su_name").value.trim();
  let e=document.getElementById("su_email").value.trim();
  let p=document.getElementById("su_pass").value.trim();
  if(!n||!e||!p){alert("Fill all fields"); return;}
  if(users.find(u=>u.email===e)){alert("Email already registered"); return;}
  let u={name:n,email:e,password:p};
  users.push(u);
  localStorage.setItem("rockUsers",JSON.stringify(users));
  currentUser=u;
  localStorage.setItem("rockCurrentUser",JSON.stringify(currentUser));
  localStorage.setItem("rockBalance", balance);
  showDashboard();
}

function login(){
  let e=document.getElementById("li_email").value.trim();
  let p=document.getElementById("li_pass").value.trim();
  if(!e||!p){alert("Fill all fields"); return;}
  let u=users.find(u=>u.email===e && u.password===p);
  if(!u){alert("Invalid credentials"); return;}
  currentUser=u;
  localStorage.setItem("rockCurrentUser",JSON.stringify(currentUser));
  showDashboard();
}

function logout(){
  currentUser=null;
  localStorage.removeItem("rockCurrentUser");
  document.getElementById("dashboard").style.display="none";
  document.getElementById("authPage").style.display="block";
}

function showDashboard(){
  document.getElementById("authPage").style.display="none";
  document.getElementById("dashboard").style.display="block";
  document.getElementById("welcome").innerText="Welcome, "+currentUser.name;
  updateBalance();
  hideAllSections();
}

function updateBalance(){
  balance = Number(localStorage.getItem("rockBalance")) || 0;
  document.getElementById("balance").innerText="₨ "+balance;
}

function hideAllSections(){
  document.getElementById("plansArea").classList.add("hidden");
  document.getElementById("depositArea").classList.add("hidden");
  document.getElementById("withdrawArea").classList.add("hidden");
  document.getElementById("shareArea").classList.add("hidden");
  document.getElementById("profileArea").classList.add("hidden");
}

function showPlans(){
  hideAllSections();
  let html='';
  plans.forEach(p=>{
    if(p.price===0){
      html+=`<div class='card'><h3>${p.name}</h3><p class='text-yellow-400'>Coming Soon</p></div>`;
    } else {
      html+=`<div class='card'><h3>${p.name}</h3><p>Price: ${p.price} PKR</p><p>Daily Profit: ${p.profit}</p><p>Days: ${p.days}</p><button class='btn btn-deposit mt-2' onclick='showDeposit(${p.price})'>Buy Now</button></div>`;
    }
  });
  let area=document.getElementById("plansArea");
  area.innerHTML=html;
  area.classList.remove("hidden");
}

function showDeposit(amount=0){
  hideAllSections();
  let html=`<h2>Deposit</h2>
  <label>Choose Method:</label>
  <select id='dep_method'>
  <option value='jazz'>JazzCash</option>
  <option value='easy'>EasyPaisa</option>
  </select>
  <p>JazzCash: 03705519562</p>
  <p>EasyPaisa: 03379827882</p>
  <input id='dep_amount' placeholder='Amount' value='${amount}'>
  <input id='dep_trx' placeholder='Transaction ID'>
  <input type='file' id='dep_proof'>
  <button class='btn btn-deposit' onclick='submitDeposit()'>Submit Deposit</button>`;
  let area=document.getElementById("depositArea");
  area.innerHTML=html;
  area.classList.remove("hidden");
}

function submitDeposit(){
  let amt=Number(document.getElementById('dep_amount').value);
  if(!amt||amt<=0){alert("Enter valid amount");return;}
  balance+=amt;
  localStorage.setItem('rockBalance',balance);
  alert('Deposit successful!');
  showDashboard();
}

function showWithdraw(){
  hideAllSections();
  let html=`<h2>Withdraw</h2>
  <label>Choose Method:</label>
  <select id='with_method'>
  <option value='jazz'>JazzCash</option>
  <option value='easy'>EasyPaisa</option>
  <option value='bank'>Bank</option>
  </select>
  <input id='with_name' placeholder='Full Name'>
  <input id='with_account' placeholder='Account / Number'>
  <input id='with_amount' placeholder='Amount'>
  <button class='btn btn-withdraw' onclick='submitWithdraw()'>Submit Withdrawal</button>`;
  let area=document.getElementById("withdrawArea");
  area.innerHTML=html;
  area.classList.remove("hidden");
}

function submitWithdraw(){
  let amt=Number(document.getElementById('with_amount').value);
  if(!amt||amt<=0){alert("Enter valid amount");return;}
  if(amt>balance){alert("Insufficient balance");return;}
  balance-=amt;
  localStorage.setItem('rockBalance',balance);
  alert('Withdrawal requested successfully!');
  showDashboard();
}
</script></body>
</html>
