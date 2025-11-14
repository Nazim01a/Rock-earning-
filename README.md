<Hi>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>🚀 Rock Earn Pro</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body {
    font-family:'Segoe UI',sans-serif;
    background: linear-gradient(135deg,#0f1123,#1c1f3b);
    color:white;
    margin:0;
    padding:15px;
    overflow-x:hidden;
}
.glass-card {
    background: rgba(255,255,255,0.05);
    backdrop-filter: blur(12px);
    border-radius:20px;
    padding:20px;
    margin-bottom:15px;
    position:relative;
    transition: transform 0.3s, box-shadow 0.3s;
}
.glass-card:hover {
    transform: scale(1.03);
    box-shadow: 0 15px 30px rgba(0,0,0,0.7);
}
.btn {
    background: linear-gradient(90deg,#5d5fef,#00ffe0);
    padding:12px 15px;
    border-radius:12px;
    font-weight:bold;
    width:100%;
    margin-top:10px;
    transition: all 0.3s, box-shadow 0.3s;
    position:relative;
    overflow:hidden;
}
.btn::after {
    content:'';
    position:absolute;
    top:0; left:-75%;
    width:50%; height:100%;
    background:rgba(255,255,255,0.2);
    transform:skewX(-20deg);
    transition: all 0.5s;
}
.btn:hover::after { left:150%; }
.btn:hover { transform:scale(1.05); opacity:0.9; box-shadow:0 0 15px #00ffe0,0 0 25px #5d5fef;}
.btn-disabled { background:#555555; cursor:not-allowed; opacity:0.6; }
.copy-btn {
    background:#334155;
    padding:3px 8px;
    border-radius:6px;
    margin-left:8px;
    font-size:13px;
    cursor:pointer;
}
.hidden { display:none; }
input, select {
    background: rgba(255,255,255,0.05);
    border:none;
    padding:10px;
    border-radius:10px;
    margin-top:8px;
    width:100%;
    color:white;
}
h1,h2,h3 { text-align:center; }
.spark {
    position:absolute;
    width:6px; height:6px;
    background: #00ffe0;
    border-radius:50%;
    animation:sparkAnim 1s linear infinite;
}
@keyframes sparkAnim {
    0% {opacity:1; transform:translate(0,0);}
    50% {opacity:0.5; transform:translate(5px,-5px);}
    100% {opacity:0; transform:translate(-5px,5px);}
}
</style>
</head>
<body>

<h1 class="text-4xl font-bold mb-5">🚀 Rock Earn Pro Dashboard</h1>

<!-- BALANCE -->
<div class="glass-card">
  <h2 class="text-2xl font-bold">Available Balance</h2>
  <p id="balance" class="text-3xl font-semibold mt-2">₨ 0</p>
</div>

<!-- PLANS -->
<div id="plansArea" class="grid grid-cols-1 gap-4 mb-5"></div>

<!-- DEPOSIT -->
<div id="depositArea" class="glass-card hidden"></div>

<!-- WITHDRAW -->
<div id="withdrawArea" class="glass-card hidden"></div>

<!-- DAILY PROFIT -->
<div id="profitArea" class="glass-card hidden">
  <h2 class="text-2xl font-bold mb-2">Daily Profit</h2>
  <div id="profitList" class="mb-2"></div>
  <p>Total Profit: ₨ <span id="totalProfit">0</span></p>
</div>

<audio id="clickSound" src="https://freesound.org/data/previews/256/256113_3263906-lq.mp3"></audio>

<script>
// BALANCE & PLANS
let balance = Number(localStorage.getItem("rockBalance")) || 0;
let boughtPlans = JSON.parse(localStorage.getItem("rockBoughtPlans")) || [];
document.getElementById("balance").innerText = "₨ "+balance;

// ACTIVE 13 PLANS
const activePlans=[
  {name:"Starter Plan",price:180,profit:30},
  {name:"Basic Plan",price:350,profit:70},
  {name:"Silver Plan",price:800,profit:150},
  {name:"Gold Plan",price:1500,profit:300},
  {name:"Pro Plan",price:2500,profit:550},
  {name:"Diamond Plan",price:4000,profit:900},
  {name:"Elite Plan",price:7000,profit:1600},
  {name:"Titan Plan",price:15000,profit:3500},
  {name:"Master Plan",price:20000,profit:4800},
  {name:"Infinity Plan",price:25000,profit:6200},
  {name:"Ultra Plan",price:30000,profit:7800},
  {name:"Super Plan",price:35000,profit:9500},
  {name:"Mega Plan",price:40000,profit:11500}
];

// COMING SOON 5 PLANS
const comingSoonPlans=[
  {name:"Legendary Plan",price:50000,profit:15000,days:70},
  {name:"Supreme Plan",price:70000,profit:22000,days:75},
  {name:"Infinity Plus",price:100000,profit:35000,days:80},
  {name:"Ultra Elite",price:150000,profit:50000,days:85},
  {name:"Mega Titan",price:200000,profit:75000,days:90}
];

// RENDER ACTIVE PLANS
const plansArea=document.getElementById("plansArea");
activePlans.forEach(p=>{
  let card=document.createElement("div");
  card.className="glass-card";
  card.innerHTML=`
    <h3 class="text-xl font-bold">${p.name}</h3>
    <p class="mt-2">Price: ₨ ${p.price}</p>
    <p class="mt-1">Profit: ₨ ${p.profit}</p>
    <button class="btn mt-2" onclick="buyPlan('${p.name}',${p.price},${p.profit})">Buy Now</button>
  `;
  plansArea.appendChild(card);
});

// RENDER COMING SOON PLANS
comingSoonPlans.forEach(p=>{
  let card=document.createElement("div");
  card.className="glass-card";
  card.innerHTML=`
    <h3 class="text-xl font-bold">${p.name} (Coming Soon)</h3>
    <p class="mt-2">Price: ₨ ${p.price}</p>
    <p>Profit: ₨ ${p.profit}</p>
    <p>Duration: ${p.days} days</p>
    <button class="btn-disabled mt-2">Buy Now</button>
  `;
  plansArea.appendChild(card);
});

// COPY FUNCTION
function copyText(id){
  let text=document.getElementById(id).innerText;
  navigator.clipboard.writeText(text);
  alert("Copied: "+text);
}

// HIDE ALL
function hideAll(){
  document.getElementById("depositArea").classList.add("hidden");
  document.getElementById("withdrawArea").classList.add("hidden");
  document.getElementById("profitArea").classList.add("hidden");
}

// PLAY CLICK SOUND
function playClick(){ document.getElementById("clickSound").play(); }

// BUY PLAN → AUTO DEPOSIT
function buyPlan(name,price,profit){
  playClick();
  hideAll();
  const d=document.getElementById("depositArea");
  d.classList.remove("hidden");
  d.innerHTML=`
    <h2 class="text-xl font-bold mb-3">Deposit for ${name}</h2>
    <p>JazzCash: <span id="jazNum">03705519562</span> 
       <span class="copy-btn" onclick="copyText('jazNum')">Copy</span></p>
    <p>EasyPaisa: <span id="easyNum">03379827882</span> 
       <span class="copy-btn" onclick="copyText('easyNum')">Copy</span></p>
    <input type="number" id="depositAmount" value="${price}" readonly>
    <input type="text" id="depositTxn" placeholder="Transaction ID">
    <input type="file" id="depositProof">
    <button class="btn mt-4" onclick="submitDeposit('${name}',${price},${profit})">Submit Deposit</button>
  `;
}

// SUBMIT DEPOSIT
function submitDeposit(name,amt,profit){
  playClick();
  let txn=document.getElementById("depositTxn").value;
  let proof=document.getElementById("depositProof").files[0];
  if(!txn||!proof){alert("Sweetie 😘, Transaction ID aur Proof chahiye!");return;}
  balance+=amt;
  localStorage.setItem("rockBalance",balance);
  document.getElementById("balance").innerText="₨ "+balance;
  boughtPlans.push({name:name,profit:profit});
  localStorage.setItem("rockBoughtPlans",JSON.stringify(boughtPlans));
  alert(`✅ Plan Purchased: ${name}\nDeposit: ₨ ${amt}`);
  hideAll();
}

// SHOW DAILY PROFIT
function showProfit(){
  hideAll();
  const pDiv=document.getElementById("profitArea");
  pDiv.classList.remove("hidden");
  const list=document.getElementById("profitList");
  list.innerHTML="";
  let total=0;
  boughtPlans.forEach(p=>{
    let div=document.createElement("div");
    div.innerHTML=`${p.name}: ₨ ${p.profit}`;
    list.appendChild(div);
    total+=p.profit;
  });
  document.getElementById("totalProfit").innerText=total;
}

// AUTO DAILY PROFIT EVERY 10s
setInterval(()=>{
  boughtPlans.forEach(p=>balance+=p.profit);
  document.getElementById("balance").innerText="₨ "+balance;
  localStorage.setItem("rockBalance",balance);
},10000);

// SHOW WITHDRAW
function showWithdraw(){
  playClick();
  hideAll();
  const w=document.getElementById("withdrawArea");
  w.classList.remove("hidden");
  w.innerHTML=`
    <h2 class="text-xl font-bold mb-3">Withdraw Funds</h2>
    <input type="text" id="withName" placeholder="Full Name">
    <input type="text" id="withAcc" placeholder="Account Number">
    <input type="number" id="withAmt" placeholder="Amount">
    <select id="withMethod">
      <option value="jazz">JazzCash</option>
      <option value="easy">EasyPaisa</option>
      <option value="bank">Bank</option>
    </select>
    <input type="text" id="withTxn" placeholder="Transaction ID">
    <input type="file" id="withProof">
    <button class="btn mt-4" onclick="submitWithdraw()">Submit Withdraw</button>
  `;
}

// SUBMIT WITHDRAW
function submitWithdraw(){
  playClick();
  let name=document.getElementById("withName").value;
  let acc=document.getElementById("withAcc").value;
  let amt=Number(document.getElementById("withAmt").value);
  let method=document.getElementById("withMethod").value;
  let txn=document.getElementById("withTxn").value;
  let proof=document.getElementById("withProof").files[0];
  if(!name||!acc||!amt||!txn||!proof||amt>balance){alert("Sweetie 😘, Fill all fields correctly & ensure enough balance"); return;}
  balance-=amt;
  localStorage.setItem("rockBalance",balance);
  document.getElementById("balance").innerText="₨ "+balance;
  alert(`💸 Withdrawal Requested\nName:${name}\nAccount:${acc}\nAmount:₨${amt}\nMethod:${method}`);
  hideAll();
}
</script>

</body>
</html>
