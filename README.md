<ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Pro — Ultimate Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;background:#0c0f1f;color:white;}
.card{background:#15172b;border-radius:18px;padding:20px;box-shadow:0 0 20px #0005;transition:0.3s;}
.card:hover{transform:scale(1.02);}
.btn{background:#4f46e5;padding:10px 16px;border-radius:10px;font-weight:600;transition:0.2s;}
.btn:hover{background:#4338ca;}
input,select{padding:10px;border-radius:10px;width:100%;margin-bottom:10px;background:#1a1c2b;color:white;border:none;}
.icon-btn{display:flex;align-items:center;gap:8px;}
#sidePanel{position:fixed;top:0;right:-400px;width:380px;height:100vh;background:#0f172a;padding:20px;transition:0.4s;overflow-y:auto;z-index:999;}
#sidePanel.active{right:0;}
.plan-list{margin-top:20px;}
.plan-item{border:1px solid #333;padding:10px;margin-bottom:8px;border-radius:10px;}
</style>
</head>
<body class="p-4"><!-- AUTH --><div id="authBox" class="card mb-6">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" />
  <input id="authEmail" placeholder="Email" />
  <input id="authPass" type="password" placeholder="Password" />
  <button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
  <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div><!-- WELCOME --><div id="welcomeBox" style="display:none;" class="card mb-4 p-4 text-center"></div><!-- DASHBOARD ICONS --><div id="dashboard" style="display:none;">
  <div class="grid grid-cols-3 gap-4 mb-6">
    <button class="icon-btn btn" onclick="openPanel('profile')"><i class="fa fa-user"></i> Profile</button>
    <button class="icon-btn btn bg-green-600" onclick="openPanel('deposit')"><i class="fa fa-wallet"></i> Deposit</button>
    <button class="icon-btn btn bg-yellow-600" onclick="openPanel('withdraw')"><i class="fa fa-money-bill"></i> Withdraw</button>
    <button class="icon-btn btn bg-purple-600" onclick="openPanel('activity')"><i class="fa fa-list"></i> Activity</button>
    <button class="icon-btn btn bg-pink-600" onclick="openPanel('plans')"><i class="fa fa-gem"></i> Plans</button>
    <button class="icon-btn btn bg-gray-600" onclick="openPanel('company')"><i class="fa fa-building"></i> Company</button>
    <button class="icon-btn btn bg-red-600" onclick="openPanel('settings')"><i class="fa fa-cog"></i> Settings</button>
    <button class="icon-btn btn bg-blue-600" onclick="openPanel('referral')"><i class="fa fa-link"></i> Referral</button>
    <button class="icon-btn btn bg-indigo-600" onclick="openPanel('leaderboard')"><i class="fa fa-trophy"></i> Leaderboard</button>
    <button class="icon-btn btn bg-teal-600" onclick="openPanel('profit')"><i class="fa fa-chart-line"></i> Daily Profit</button>
    <button class="icon-btn btn bg-orange-600" onclick="openPanel('support')"><i class="fa fa-headset"></i> Support</button>
    <button class="icon-btn btn bg-cyan-600" onclick="openPanel('faq')"><i class="fa fa-question-circle"></i> FAQ</button>
  </div>
</div><!-- SIDE PANEL --><div id="sidePanel"></div><script>
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(currentUser){openDashboard();}

function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return alert('Fill all fields');if(!validateEmail(e))return alert('Invalid email');if(users.find(u=>u.email===e))return alert('Email already registered');let u={name:n,email:e,pass:p,plans:[],referrals:[],balance:0,profit:0,notifications:true};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return alert('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('welcomeBox').style.display='block';document.getElementById('dashboard').style.display='block';document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💖</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p><button onclick="logoutUser()" class='btn bg-red-600 w-full mt-2'><i class='fa fa-sign-out-alt'></i> Logout</button>`;}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

// PLANS DATA
let plans=[{name:'Starter',amount:180,daily:20},{name:'Basic',amount:350,daily:50},{name:'Pro',amount:500,daily:80},{name:'Premium',amount:1000,daily:180},{name:'Ultra',amount:2500,daily:450},{name:'Mega',amount:5000,daily:950},{name:'Elite',amount:7000,daily:1350},{name:'Mega Booster',amount:10000,daily:1500,coming:true},{name:'Ultra Pro',amount:15000,daily:2300,coming:true},{name:'Crypto Miner',amount:20000,daily:3500,coming:true}];

function openPanel(type,amount=0,name=''){let panel=document.getElementById('sidePanel');panel.classList.add('active');panel.innerHTML='';
if(type==='plans'){
  panel.innerHTML='<h2>All Plans</h2><div class="plan-list">'+plans.map(p=>`<div class='plan-item'><b>${p.name}</b> - ${p.amount} PKR - Daily Profit: ${p.daily} PKR ${p.coming?'(Coming Soon)':''}<button class='btn mt-1' onclick='buyPlan(${p.amount},"${p.name}")'>Buy Now</button></div>`).join('')+'</div>';
}
if(type==='profile'){panel.innerHTML=`<h2>Profile</h2><p>Name: ${currentUser.name}</p><p>Email: ${currentUser.email}</p><p>Plans Bought: ${currentUser.plans.map(p=>p.name).join(', ')||'None'}</p><p>Notifications: ${currentUser.notifications?'On':'Off'}</p>`;}
if(type==='deposit'){panel.innerHTML=`<h2>Deposit</h2><p>Plan Selected: ${name}</p><p>Amount: ${amount} PKR</p><p>JazzCash: 03705519562 <button onclick="copyText('03705519562')" class='btn'>Copy</button></p><p>EasyPaisa: 03379827882 <button onclick="copyText('03379827882')" class='btn'>Copy</button></p><input type='file' id='proofFile' class='mt-2'/><button class='btn mt-2 w-full' onclick='uploadProof()'>Upload Proof</button><input type='text' placeholder='Transaction ID' class='mt-2 w-full p-2 rounded bg-white/20'/><button class='btn mt-2 w-full' onclick='confirmDeposit()'>Confirm Deposit</button>`;}
if(type==='withdraw'){panel.innerHTML=`<h2>Withdraw</h2><input placeholder='Amount'/><select><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input placeholder='Account Number'/><button class='btn w-full mt-2'>Withdraw</button>`;}
if(type==='activity'){panel.innerHTML=`<h2>Activity</h2><ul>${currentUser.plans.map(p=>`<li>${p.name} - ${p.amount} PKR</li>`).join('')}</ul>`;}
if(type==='company'){panel.innerHTML=`<h2>Company Details</h2><p>Founded: 2018</p><p>Partnered: Binance & Crypto</p><p>Address: 3500 Deer Creek Road, Palo Alto, CA, USA</p>`;}
if(type==='settings'){panel.innerHTML=`<h2>Settings</h2><input placeholder='Change Name'/><input placeholder='Change Email'/><input type='password' placeholder='Change Password'/><label><input type='checkbox' ${currentUser.notifications?'checked':''}/> Enable Notifications</label><button class='btn w-full mt-2'>Save</button>`;}
if(type==='referral'){panel.innerHTML=`<h2>Referral</h2><p>Your link: <b>https://rockearnpro.com/ref/${currentUser.email}</b> <button onclick="copyText('https://rockearnpro.com/ref/${currentUser.email}')" class='btn'>Copy</button></p>`;}
if(type==='leaderboard'){panel.innerHTML=`<h2>Leaderboard</h2><ol>${users.sort((a,b)=>b.plans.length-a.plans.length).slice(0,5).map(u=>`<li>${u.name} - ${u.plans.length} plans</li>`).join('')}</ol>`;}
if(type==='profit'){panel.innerHTML=`<h2>Daily Profit</h2><p>Your total profit: ${currentUser.profit} PKR</p><button class='btn mt-2' onclick='calculateProfit()'>Update Profit</button>`;}
if(type==='support'){panel.innerHTML=`<h2>Support</h2><p>Email: support@rockearnpro.com</p><p>Live Chat: Coming Soon</p>`;}
if(type==='faq'){panel.innerHTML=`<h2>FAQ</h2><ul><li>How to deposit?</li><li>How to withdraw?</li><li>How to refer friends?</li></ul>`;}}

function buyPlan(amount,name){if(!currentUser.plans)currentUser.plans=[];currentUser.plans.push({name:name,amount:amount,daily:plans.find(p=>p.name===name).daily});currentUser.balance+=amount;localStorage.setItem('reCurrent',JSON.stringify(currentUser));openPanel('deposit',amount,name);}
function copyText(val){navigator.clipboard.writeText(val);alert('Copied: '+val);}
function uploadProof(){let fileInput=document.getElementById('proofFile');if(!fileInput||!fileInput.files[0])return alert('Select a file first');let fileName=fileInput.files[0].name;localStorage.setItem('proofFileName',fileName);alert('Proof uploaded: '+fileName);}
function confirmDeposit(){alert('Deposit confirmed!');}
function calculateProfit(){let total=0;currentUser.plans.forEach(p=>total+=p.daily);currentUser.profit=total;localStorage.setItem('reCurrent',JSON.stringify(currentUser));alert('Profit updated: '+total+' PKR');openPanel('profit');}
</script></body>
</html>
