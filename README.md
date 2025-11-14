<ROCK><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Official — Pro Ultimate</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;background:#0d0f1d;color:white;}
.card{background:#15172b;border-radius:18px;padding:20px;box-shadow:0 0 20px #0005;transition:0.3s;}
.card:hover{transform:scale(1.02);}
.btn{background:#4f46e5;padding:10px 16px;border-radius:10px;font-weight:600;transition:0.2s;}
.btn:hover{background:#4338ca;}
input,select{padding:10px;border-radius:10px;width:100%;margin-bottom:10px;background:#1a1c2b;color:white;border:none;}
.icon-btn{display:flex;align-items:center;gap:8px;}
</style>
</head>
<body class="p-4"><!-- AUTH BOX --><div id="authBox" class="card mb-6">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" />
  <input id="authEmail" placeholder="Email" />
  <input id="authPass" type="password" placeholder="Password" /><button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button> <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>

  <p onclick="forgotPassword()" class="text-blue-400 text-center mt-3 cursor-pointer"><i class="fa fa-key"></i> Forgot Password?</p>
</div><!-- WELCOME + LOGOUT --><div id="welcomeBox" style="display:none;" class="card mb-4 p-4 text-center"></div><!-- DASHBOARD --><div id="dashboard" style="display:none;">  <!-- ICON MENU -->  <div class="grid grid-cols-3 gap-4 mb-6">
    <button class="icon-btn btn" onclick="openPanel('profile')"><i class="fa fa-user"></i> Profile</button>
    <button class="icon-btn btn bg-green-600" onclick="openPanel('deposit')"><i class="fa fa-wallet"></i> Deposit</button>
    <button class="icon-btn btn bg-yellow-600" onclick="openPanel('withdraw')"><i class="fa fa-money-bill"></i> Withdraw</button>
    <button class="icon-btn btn bg-purple-600" onclick="openPanel('activity')"><i class="fa fa-list"></i> Activity</button>
    <button class="icon-btn btn bg-gray-600" onclick="openPanel('company')"><i class="fa fa-building"></i> Company</button>
    <button class="icon-btn btn bg-red-600" onclick="openPanel('settings')"><i class="fa fa-cog"></i> Settings</button>
  </div>  <!-- PLANS -->  <h2 class="text-xl font-bold mb-3">Our Plans</h2>
  <div class="grid grid-cols-1 gap-4" id="planList"></div>  <!-- SIDE PANEL -->  <div id="sidePanel" class="card mt-6 hidden"></div>
</div><script>
let users = JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser = JSON.parse(localStorage.getItem('reCurrent'))||null;

if(currentUser){openDashboard();} // auto-login after refresh

function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return alert('Fill all fields');if(!validateEmail(e))return alert('Invalid email');if(users.find(u=>u.email===e))return alert('Email already registered');let u={name:n,email:e,pass:p};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return alert('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function forgotPassword(){let e=prompt('Enter your email');if(!validateEmail(e))return alert('Invalid email');let u=users.find(x=>x.email===e);if(!u)return alert('Email not found');alert('Your password: '+u.pass);}

function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('welcomeBox').style.display='block';document.getElementById('dashboard').style.display='block';document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💖</h2><button onclick="logoutUser()" class='btn bg-red-600 w-full mt-2'><i class='fa fa-sign-out-alt'></i> Logout</button>`;loadPlans();}
function logoutUser(){localStorage.removeItem('reCurrent');location.reload();}

// PLANS DATA
let plans=[{name:'Starter',amount:180},{name:'Basic',amount:350},{name:'Pro',amount:500},{name:'Premium',amount:1000},{name:'Ultra',amount:2500},{name:'Mega',amount:5000},{name:'Elite',amount:7000}];
function loadPlans(){let box=document.getElementById('planList');box.innerHTML='';plans.forEach(p=>{box.innerHTML+=`<div class='card'><h3>${p.name}</h3><p>Price: ${p.amount} PKR</p><button class='btn' onclick="openPanel('deposit',${p.amount})"><i class='fa fa-cart-plus'></i> Buy Now</button></div>`;});}

function openPanel(type,amount){let panel=document.getElementById('sidePanel');panel.classList.remove('hidden');panel.innerHTML='';if(type==='profile'){panel.innerHTML=`<h2>Profile</h2><p>Name: ${currentUser.name}</p><p>Email: ${currentUser.email}</p>`;}
if(type==='deposit'){panel.innerHTML=`<h2>Deposit</h2><p>Amount: ${amount} PKR</p><p>JazzCash: 03705519562 <button onclick="copyText('03705519562')" class='btn'>Copy</button></p><p>EasyPaisa: 03379827882 <button onclick="copyText('03379827882')" class='btn'>Copy</button></p><input type='text' placeholder='Transaction ID' /><button class='btn w-full mt-2'>Upload Proof</button>`;}
if(type==='withdraw'){panel.innerHTML=`<h2>Withdraw</h2><input placeholder='Amount'/><select><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input placeholder='Account Number'/><button class='btn w-full mt-2'>Withdraw</button>`;}
if(type==='activity'){panel.innerHTML=`<h2>Activity</h2><ul><li>No activity yet</li></ul>`;}
if(type==='company'){panel.innerHTML=`<h2>Company Details</h2><p>Founded: 2018</p><p>Partnered: Binance & Crypto</p><p>Address: 3500 Deer Creek Road, Palo Alto, CA, USA</p>`;}
if(type==='settings'){panel.innerHTML=`<h2>Settings</h2><input placeholder='Change Name'/><input placeholder='Change Email'/><input type='password' placeholder='Change Password'/><label><input type='checkbox'/> Enable Notifications</label><button class='btn w-full mt-2'>Save</button>`;}}

function copyText(val){navigator.clipboard.writeText(val);alert('Copied: '+val);}
</script></body>
</html>
