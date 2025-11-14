<Hi>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Pro Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body {
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(145deg,#0d0f1d,#1b1e36);
    color: white;
    margin: 0;
}
.logo {
    font-size: 38px;
    font-weight: bold;
    text-align: center;
    margin: 30px 0;
    background: linear-gradient(90deg, #5d5fef, #00ffe0);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
.card {
    background: #1f223e;
    border-radius: 22px;
    padding: 25px;
    box-shadow: 0 18px 35px rgba(0,0,0,0.7);
    margin-bottom: 20px;
    transition: transform 0.3s, box-shadow 0.3s;
}
.card:hover { transform: translateY(-10px); box-shadow: 0 25px 50px rgba(0,0,0,0.8); }
.btn {
    padding: 16px 22px;
    border-radius: 18px;
    font-weight: 600;
    cursor: pointer;
    display: inline-block;
    margin-top: 14px;
    width: 100%;
    text-align: center;
    transition: all 0.3s;
    font-size: 16px;
}
.btn:hover { opacity: 0.9; transform: scale(1.05); }
.btn-deposit { background: linear-gradient(90deg, #1dd11d, #00ffb8); color: black; }
.btn-withdraw { background: linear-gradient(90deg, #f5b700, #ff6b00); color: black; }
.btn-logout { background: linear-gradient(90deg, #ff4d4d, #ff1a1a); }
.dashboard-icons {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(140px,1fr));
    gap: 18px;
    margin-top: 25px;
}
.dashboard-icons div {
    padding: 18px;
    background: #2d314d;
    border-radius: 18px;
    cursor: pointer;
    transition: 0.3s;
    text-align: center;
    font-size: 20px;
    font-weight: 500;
}
.dashboard-icons div:hover { background: #3f4368; transform: translateY(-6px); }
input, select { background: #21243f; border: none; padding: 14px; width: 100%; border-radius: 14px; color: white; margin-top: 12px; transition: 0.3s; font-size: 15px; }
input:focus, select:focus { outline: 2px solid #5d5fef; }
.scroll { max-height: 420px; overflow-y: auto; }
.profile-card { padding: 22px; background: #2d314d; border-radius: 18px; box-shadow: 0 15px 35px rgba(0,0,0,0.6); }
h2,h3 { font-weight: 700; }
@media(max-width:640px){ .dashboard-icons {grid-template-columns: repeat(2,1fr);} .logo {font-size: 32px;} }
</style>
</head>
<body>
<div class="logo">🚀 Rock Earn Pro</div>

<!-- AUTH PAGE -->
<div id="authPage" class="p-5 max-w-md mx-auto">
    <div class="card">
        <h2 class="text-3xl mb-5 text-center">Sign Up</h2>
        <input id="su_name" placeholder="Username">
        <input id="su_email" placeholder="Email">
        <input id="su_pass" type="password" placeholder="Password">
        <button class="btn btn-deposit mt-4" onclick="signup()">Sign Up</button>
    </div>
    <div class="card mt-6">
        <h2 class="text-3xl mb-5 text-center">Login</h2>
        <input id="li_email" placeholder="Email">
        <input id="li_pass" type="password" placeholder="Password">
        <button class="btn btn-deposit mt-4" onclick="login()">Login</button>
    </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="hidden p-5 max-w-6xl mx-auto">
    <p id="welcome" class="text-2xl font-semibold mb-5"></p>
    <p class="text-lg mb-4">Available Balance: <span id="balance">₨ 0</span></p>
    
    <div class="dashboard-icons">
        <div onclick="showPlans()">📈 Plans</div>
        <div onclick="showDeposit()">💰 Deposit</div>
        <div onclick="showWithdraw()">🏧 Withdraw</div>
        <div onclick="showProfit()">📊 Daily Profit</div>
        <div onclick="showAccountDetails()">💳 Account Details</div>
        <div onclick="showSettings()">⚙️ Settings</div>
        <div onclick="showShare()">🔗 Share Link</div>
        <div onclick="showProfile()">👤 Profile</div>
        <div onclick="showCompany()">🏢 Company Details</div>
        <div onclick="logout()">🚪 Logout</div>
    </div>

    <!-- SECTIONS -->
    <div id="plansArea" class="scroll hidden mt-6 grid gap-5 md:grid-cols-2 lg:grid-cols-3"></div>
    
    <div id="depositArea" class="hidden card mt-6">
      <h3 class="text-xl font-bold mb-4">Deposit Funds</h3>
      <select id="depositMethod">
        <option value="jazz">JazzCash - 03705519562</option>
        <option value="easy">EasyPaisa - 03379827882</option>
      </select>
      <input type="number" id="depositAmount" placeholder="Amount">
      <input type="text" id="depositTransaction" placeholder="Transaction ID / Reference">
      <input type="file" id="depositProof">
      <button class="btn btn-deposit mt-3" onclick="submitDeposit()">Submit Deposit</button>
    </div>

    <div id="withdrawArea" class="hidden card mt-6">
      <h3 class="text-xl font-bold mb-4">Withdraw Funds</h3>
      <input id='withName' placeholder='Full Name'>
      <input id='withAccount' placeholder='Account Number'>
      <input id='withAmount' placeholder='Amount'>
      <select id='withMethod'>
        <option value='jazz'>JazzCash</option>
        <option value='easy'>EasyPaisa</option>
        <option value='bank'>Bank</option>
      </select>
      <input type="text" id="withTxn" placeholder="Transaction ID / Reference">
      <input type="file" id="withProof">
      <button class='btn btn-withdraw mt-3' onclick='submitWithdraw()'>Submit Withdraw</button>
    </div>

    <div id="profitArea" class="scroll hidden mt-6">
      <h3 class="text-xl font-bold mb-4">Daily Profit</h3>
      <div id="profitList" class="grid gap-3"></div>
    </div>

    <div id="accountArea" class="hidden card mt-6">
      <h3 class="text-xl font-bold mb-3">Account Details</h3>
      <p>Name: <span id="accName"></span></p>
      <p>Email: <span id="accEmail"></span></p>
    </div>

    <div id="settingsArea" class="hidden card mt-6">
      <h3 class="text-xl font-bold mb-3">Settings</h3>
      <p>Settings content here...</p>
    </div>

    <div id="shareArea" class="hidden card mt-6">
      <h3 class="text-xl font-bold mb-3">Share Link</h3>
      <button class="btn btn-deposit" onclick="copyLink()">Copy Your Referral Link</button>
    </div>

    <div id="profileArea" class="hidden card mt-6 profile-card">
      <h3 class="text-xl font-bold mb-3">Profile</h3>
      <p>Profile info here...</p>
    </div>

    <div id="companyArea" class="hidden card mt-6">
      <h3 class="text-xl font-bold mb-3">Company Details</h3>
      <p>Company info here...</p>
    </div>
</div>

<script>
// USER DATA
let users = JSON.parse(localStorage.getItem("rockUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("rockCurrentUser")) || null;
let balance = Number(localStorage.getItem("rockBalance")) || 0;
let boughtPlans = JSON.parse(localStorage.getItem("rockBoughtPlans")) || [];

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

// ONLOAD
window.onload = function(){ if(currentUser){ showDashboard(); } updateBalance(); renderAccount(); startDailyProfit(); };

// AUTH
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

// DASHBOARD
function showDashboard(){
    document.getElementById("authPage").style.display="none";
    document.getElementById("dashboard").style.display="block";
    document.getElementById("welcome").innerText="Welcome to Rock Earn, "+currentUser.name;
    updateBalance(); hideAllSections();
}
function hideAllSections(){
    document.querySelectorAll('#plansArea,#depositArea,#withdrawArea,#profitArea,#accountArea,#settingsArea,#shareArea,#profileArea,#companyArea').forEach(s=>s.classList.add('hidden'));
}
function updateBalance(){ balance = Number(localStorage.getItem("rockBalance")) || 0; document.getElementById("balance").innerText="₨ "+balance.toFixed(2); }
function renderAccount(){ if(currentUser){ document.getElementById("accName").innerText=currentUser.name; document.getElementById("accEmail").innerText=currentUser.email; }}

// DEPOSIT & WITHDRAW
function submitDeposit(){
    let method = document.getElementById('depositMethod').value;
    let amt = Number(document.getElementById('depositAmount').value);
    let txn = document.getElementById('depositTransaction').value.trim();
    let proof = document.getElementById('depositProof').files[0];
    if(!amt || !txn || !proof){ alert("Please fill all fields and upload proof"); return; }
    balance += amt;
    localStorage.setItem('rockBalance', balance);
    updateBalance();
    alert(`✅ Deposit Submitted!\nMethod: ${method}\nAmount: ₨ ${amt}\nTransaction ID: ${txn}`);
    hideAllSections();
}
function submitWithdraw(){
    let amt=Number(document.getElementById('withAmount').value);
    let name=document.getElementById('withName').value.trim();
    let account=document.getElementById('withAccount').value.trim();
    let method=document.getElementById('withMethod').value;
    let txn=document.getElementById('withTxn').value.trim();
    let proof=document.getElementById('withProof').files[0];
    if(!amt || !name || !account || !txn || !proof || amt>balance){ alert('Please fill all fields correctly and ensure sufficient balance'); return; }
    balance-=amt; localStorage.setItem('rockBalance',balance); updateBalance();
    alert(`💸 Withdrawal Requested!\nName: ${name}\nAccount: ${account}\nAmount: ₨ ${amt}\nTransaction ID: ${txn}\nMethod: ${method}`);
    hideAllSections();
}

// PLANS
function showPlans(){
    hideAllSections();
    const plansDiv = document.getElementById("plansArea");
    plansDiv.innerHTML = "";
    plans.forEach(plan => {
        let card = document.createElement("div");
        card.className = "card";
        card.innerHTML = `
            <h3 class="text-xl font-bold mb-2">${plan.name}</h3>
            <p>Price: ₨ ${plan.price}</p>
            <p>Profit: ₨ ${plan.profit}</p>
            <p>Duration: ${plan.days} days</p>
            <button class="btn btn-deposit mt-3" onclick="buyPlan('${plan.name}', ${plan.price})">Buy Now</button>
        `;
        plansDiv.appendChild(card);
    });
    plansDiv.classList.remove('hidden');
}
function buyPlan(name, price){
    if(price > balance){ alert("Insufficient balance! Please deposit."); return; }
    balance -= price;
    localStorage.setItem('rockBalance', balance);
    updateBalance();
    boughtPlans.push(name);
    localStorage.setItem('rockBoughtPlans', JSON.stringify(boughtPlans));
    alert(`✅ Plan purchased: ${name}\nAmount deducted: ₨ ${price}`);
}

// DAILY PROFIT
function showProfit(){
    hideAllSections();
    const profitDiv = document.getElementById("profitList");
    profitDiv.innerHTML = "";
    boughtPlans.forEach((plan, idx) => {
        let profit = plans.find(p => p.name === plan)?.profit || 0;
        let card = document.createElement("div");
        card.className = "card flex justify-between items-center";
        card.innerHTML = `<span>${plan}</span> <span>₨ ${profit}</span>`;
        profitDiv.appendChild(card);
    });
    document.getElementById("profitArea").classList.remove('hidden');
}
function startDailyProfit(){
    setInterval(()=>{
        boughtPlans.forEach(planName => {
            let plan = plans.find(p=>p.name===planName);
            if(plan){
                balance += plan.profit;
                localStorage.setItem('rockBalance', balance);
                updateBalance();
            }
        });
    }, 86400000);
}

// OTHER SECTIONS
function showAccountDetails(){ hideAllSections(); document.getElementById("accountArea").classList.remove('hidden'); }
function showSettings(){ hideAllSections(); document.getElementById("settingsArea").classList.remove('hidden'); }
function showShare(){ hideAllSections(); copyLink(); }
function showProfile(){ hideAllSections(); document.getElementById("profileArea").classList.remove('hidden'); }
function showCompany(){ hideAllSections(); document.getElementById("companyArea").classList.remove('hidden'); }
function showDeposit(){ hideAllSections(); document.getElementById("depositArea").classList.remove('hidden'); }
function copyLink(){ navigator.clipboard.writeText(window.location.href + "?ref="+encodeURIComponent(currentUser.email)); alert("Referral link copied!"); }
</script>
</body>
</html>
