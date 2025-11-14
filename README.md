<ROCK><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Official — Pro Ultimate</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:#0d0f1d;color:white;}
.card{background:#15172b;border-radius:18px;padding:20px;box-shadow:0 0 20px #0005;transition:0.3s;}
.card:hover{transform:scale(1.02);}
.btn{background:#4f46e5;padding:10px 16px;border-radius:10px;font-weight:600;transition:0.2s;}
.btn:hover{background:#4338ca;}
input,select{padding:10px;border-radius:10px;width:100%;margin-bottom:10px;background:#1a1c2b;color:white;border:none;}
</style>
</head>
<body class="p-4"><!-- AUTH BOX --><div id="authBox" class="card mb-6">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" />
  <input id="authEmail" placeholder="Email" />
  <input id="authPass" type="password" placeholder="Password" /><button onclick="signupUser()" class="btn w-full mb-2">Sign Up</button> <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700">Login</button>

  <p onclick="forgotPassword()" class="text-blue-400 text-center mt-3 cursor-pointer">Forgot Password?</p>
</div><!-- WELCOME + LOGOUT --><div id="welcomeBox" style="display:none;" class="card mb-4 p-4 text-center"></div><!-- DASHBOARD --><div id="dashboard" style="display:none;">  <!-- PLANS -->  <h2 class="text-xl font-bold mb-3">Our Plans</h2>
  <div class="grid grid-cols-1 gap-4" id="planList"></div>  <!-- DEPOSIT PANEL -->  <div id="depositPanel" class="card mt-6 hidden">
    <h2 class="text-xl font-bold mb-4">Deposit</h2>
    <p>Amount: <span id="depAmount"></span> PKR</p>
    <h3>JazzCash</h3>
    <div class="flex mb-3">
      <input id="jcNum" value="03705519562" />
      <button onclick="copyText('jcNum')" class="btn">Copy</button>
    </div>
    <h3>EasyPaisa</h3>
    <div class="flex mb-3">
      <input id="epNum" value="03379827882" />
      <button onclick="copyText('epNum')" class="btn">Copy</button>
    </div>
    <input id="trxId" placeholder="Transaction ID" />
    <button class="btn w-full mt-2" onclick="confirmDeposit()">Upload Proof</button>
  </div>  <!-- WITHDRAW -->  <div id="withdrawBox" class="card mt-6">
    <h2>Withdraw</h2>
    <input id="wdAmount" placeholder="Amount" />
    <select id="wdMethod"><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select>
    <input id="wdNum" placeholder="Account Number" />
    <button onclick="withdrawNow()" class="btn w-full bg-green-600 hover:bg-green-700">Withdraw Now</button>
  </div>  <!-- PROFILE -->  <div class="card mt-6">
    <h2>Profile</h2>
    <p id="profName"></p>
    <p id="profEmail"></p>
    <input type="file" id="profilePic" />
    <p>Referral Code: <span id="refCode">RE12345</span> <button onclick="copyText('refCode')" class="btn">Copy</button></p>
    <p>Total Deposits: <span id="totalDep">0</span> PKR</p>
    <p>Total Withdrawals: <span id="totalWd">0</span> PKR</p>
  </div>  <!-- SETTINGS -->  <div class="card mt-6">
    <h2>Settings</h2>
    <input type="text" placeholder="Change Name" />
    <input type="email" placeholder="Change Email" />
    <input type="password" placeholder="Change Password" />
    <label><input type="checkbox" id="notifToggle"> Enable Notifications</label>
    <button class="btn w-full mt-2">Save Settings</button>
  </div>  <!-- ACTIVITY -->  <div class="card mt-6">
    <h2>Activity</h2>
    <ul id="activityList"><li>No activity yet</li></ul>
  </div>  <!-- COMPANY DETAILS -->  <div class="card mt-6">
    <h2>Company Details</h2>
    <p>Founded: 2018</p>
    <p>Working with: Crypto & Binance (as soon)</p>
    <p>Location: 3500 Deer Creek Road, Palo Alto, California, USA</p>
    <p>Email: support@rockearnpro.com</p>
  </div></div><script>
let users = JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser = JSON.parse(localStorage.getItem('reCurrent'))||null;
function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return alert('Fill all fields');if(!validateEmail(e))return alert('Invalid email');if(users.find(u=>u.email===e))return alert('Email already registered');let u={name:n,email:e,pass:p};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return alert('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function forgotPassword(){let e=prompt('Enter your email');if(!validateEmail(e))return alert('Invalid email');let u=users.find(x=>x.email===e);if(!u)return alert('Email not found');alert('Your password: '+u.pass);}
function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('welcomeBox').style.display='block';document.getElementById('dashboard').style.display='block';document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💖</h2><button onclick="logoutUser()" class='btn bg-red-600 w-full mt-2'>Logout</button>`;document.getElementById('profName').innerText='Name: '+currentUser.name;document.getElementById('profEmail').innerText='Email: '+currentUser.email;loadPlans();}
function logoutUser(){localStorage.removeItem('reCurrent');location.reload();}
let plans=[{name:'Starter',amount:180},{name:'Basic',amount:350},{name:'Pro',amount:500},{name:'Premium',amount:1000},{name:'Ultra',amount:2500},{name:'Mega',amount:5000},{name:'Elite',amount:7000}];
function loadPlans(){let box=document.getElementById('planList');box.innerHTML='';plans.forEach(p=>{box.innerHTML+=`<div class='card'><h3>${p.name}</h3><p>Price: ${p.amount} PKR</p><button class='btn' onclick="openDeposit(${p.amount})">Buy Now</button></div>`;});}
function openDeposit(a){document.getElementById('depositPanel').classList.remove('hidden');document.getElementById('depAmount').innerText=a;}
function copyText(id){let i=document.getElementById(id);i.select();navigator.clipboard.writeText(i.value);alert('Copied');}
function confirmDeposit(){let t=document.getElementById('trxId').value.trim();if(!t)return alert('Enter Transaction ID');alert('Deposit Submitted');}
function withdrawNow(){let a=document.getElementById('wdAmount').value;let m=document.getElementById('wdMethod').value;let n=document.getElementById('wdNum').value;if(!a||!m||!n)return alert('Fill all fields');alert('Withdrawal Request Sent');}
</script></body>
</html>
