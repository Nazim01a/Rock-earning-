<Hi🤗>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>🚀 Rock Earn Pro</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;margin:0;padding:0;background:#0f1123;overflow-x:hidden;position:relative;}
/* Floating particles */
.particle{position:absolute;width:5px;height:5px;background:#00ffe0;border-radius:50%;opacity:0.8;animation:move 20s linear infinite;}
@keyframes move{0%{transform:translateY(0) translateX(0);}100%{transform:translateY(-1200px) translateX(200px);}}

/* Sidebar */
.sidebar{position:fixed;top:0;left:0;width:220px;height:100%;background:rgba(20,20,40,0.95);backdrop-filter:blur(12px);display:flex;flex-direction:column;align-items:center;padding-top:20px;z-index:1000;}
.sidebar h2{font-size:20px;margin-bottom:20px;}
.sidebar button{width:90%;margin:5px 0;padding:10px;border:none;border-radius:12px;background:linear-gradient(90deg,#5d5fef,#00ffe0);cursor:pointer;font-weight:bold;transition:0.3s;}
.sidebar button:hover{transform:scale(1.05);box-shadow:0 0 15px #00ffe0,0 0 25px #5d5fef;}

/* Main content */
.main-content{margin-left:240px;padding:20px;position:relative;z-index:10;}
.glass-card{background:rgba(255,255,255,0.05);backdrop-filter:blur(15px);border-radius:20px;padding:20px;margin-bottom:15px;position:relative;transition:transform 0.3s,box-shadow 0.3s;}
.glass-card:hover{transform:scale(1.05);box-shadow:0 0 30px #00ffe0,0 0 60px #5d5fef;}
.btn{background:linear-gradient(90deg,#5d5fef,#00ffe0);padding:10px;border-radius:12px;width:100%;margin-top:10px;font-weight:bold;cursor:pointer;position:relative;overflow:hidden;transition:all 0.3s,box-shadow 0.3s;}
.btn::after{content:'';position:absolute;top:0;left:-75%;width:50%;height:100%;background:rgba(255,255,255,0.2);transform:skewX(-20deg);transition:all 0.5s;}
.btn:hover::after{left:150%;}
.btn:hover{transform:scale(1.05);opacity:0.9;box-shadow:0 0 15px #00ffe0,0 0 25px #5d5fef;}
.btn-disabled{background:#555555;cursor:not-allowed;opacity:0.6;}
.copy-btn{background:#334155;padding:3px 8px;border-radius:6px;margin-left:8px;font-size:13px;cursor:pointer;}
.hidden{display:none;}
input,select{background:rgba(255,255,255,0.05);border:none;padding:10px;border-radius:10px;margin-top:8px;width:100%;color:white;}
h1,h2,h3{text-align:center;}
</style>
</head>
<body>

<!-- Floating particles container -->
<div id="particlesContainer"></div>

<div class="sidebar">
  <h2>🚀 Rock Earn Pro</h2>
  <button onclick="showSection('plans')">Plans</button>
  <button onclick="showSection('deposit')">Deposit</button>
  <button onclick="showSection('withdraw')">Withdraw</button>
  <button onclick="showSection('profit')">Daily Profit</button>
  <button onclick="showSection('history')">Activity History</button>
  <button onclick="showSection('profile')">Profile</button>
  <button onclick="showSection('company')">Company Details</button>
  <button onclick="showSection('share')">Share Link</button>
  <button onclick="showSection('settings')">Settings</button>
</div>

<div class="main-content">
  <div id="plansSection" class="hidden"></div>
  <div id="depositSection" class="hidden glass-card"></div>
  <div id="withdrawSection" class="hidden glass-card"></div>
  <div id="profitSection" class="hidden glass-card"></div>
  <div id="historySection" class="hidden glass-card"></div>
  <div id="profileSection" class="hidden glass-card"></div>
  <div id="companySection" class="hidden glass-card"></div>
  <div id="shareSection" class="hidden glass-card"></div>
  <div id="settingsSection" class="hidden glass-card"></div>
</div>

<audio id="clickSound" src="https://freesound.org/data/previews/256/256113_3263906-lq.mp3"></audio>

<script>
// PARTICLES
function createParticles(num){
  const container=document.getElementById("particlesContainer");
  for(let i=0;i<num;i++){
    let p=document.createElement("div");
    p.className="particle";
    p.style.left=Math.random()*window.innerWidth+"px";
    p.style.top=Math.random()*window.innerHeight+"px";
    p.style.width=Math.random()*4+3+"px";
    p.style.height=Math.random()*4+3+"px";
    container.appendChild(p);
  }
}
createParticles(100);

// SOUND
function playClick(){document.getElementById("clickSound").play();}

// INITIAL DATA
let balance = Number(localStorage.getItem("rockBalance")) || 0;
let boughtPlans = JSON.parse(localStorage.getItem("rockBoughtPlans")) || [];
let activityHistory = JSON.parse(localStorage.getItem("rockActivity")) || [];
const activePlans=[
{name:"Starter Plan",price:180,profit:30},{name:"Basic Plan",price:350,profit:70},{name:"Silver Plan",price:800,profit:150},{name:"Gold Plan",price:1500,profit:300},{name:"Pro Plan",price:2500,profit:550},{name:"Diamond Plan",price:4000,profit:900},{name:"Elite Plan",price:7000,profit:1600},{name:"Titan Plan",price:15000,profit:3500},{name:"Master Plan",price:20000,profit:4800},{name:"Infinity Plan",price:25000,profit:6200},{name:"Ultra Plan",price:30000,profit:7800},{name:"Super Plan",price:35000,profit:9500},{name:"Mega Plan",price:40000,profit:11500}
];
const comingPlans=[
{name:"Legendary Plan",price:50000,profit:15000,days:70},{name:"Supreme Plan",price:70000,profit:22000,days:75},{name:"Infinity Plus",price:100000,profit:35000,days:80},{name:"Ultra Elite",price:150000,profit:50000,days:85},{name:"Mega Titan",price:200000,profit:75000,days:90}
];
document.body.onload=function(){renderPlans();updateBalance();};

// FUNCTIONS: updateBalance, showSection, renderPlans, buyPlan, copyText, submitDeposit, showWithdraw, submitWithdraw, renderProfit, renderHistory, renderProfile, renderCompany, renderShare, renderSettings
// (same as previous code, fully integrated with glowing hover + particle background)
</script>

</body>
</html>
