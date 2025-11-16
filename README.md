<ROCKS>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap');

body {
  margin: 0;
  padding: 0;
  font-family: 'Orbitron', sans-serif;
  background: #111;
  color: #fff;
  overflow-x: hidden;
  position: relative;
}
body::before {
  content: '';
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: linear-gradient(270deg, #111, #222, #000, #111, #222);
  background-size: 1200% 1200%;
  animation: bgAnim 30s ease infinite;
  z-index: -1;
}
@keyframes bgAnim {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

#notif {
  position: fixed;
  top: 20px; right: 20px;
  background: #00ffff; color: #000;
  padding: 10px 20px;
  border-radius: 8px;
  opacity: 0;
  transition: 0.3s;
  font-weight: bold;
  z-index: 50;
}
#notif.show {
  opacity: 1;
}

#authBox {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  animation: fadeIn 1s;
  padding: 10px;
}
input, button {
  padding: 10px;
  margin: 5px;
  border-radius: 8px;
  border: none;
}
input { width: 220px; }
button {
  background: #00ffff;
  color: #000;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}
button:hover {
  background: #ff00ff;
  color: #fff;
}

#sidebar {
  display: none;
  position: fixed;
  left: 0; top: 0;
  width: 250px; height: 100vh;
  background: rgba(0,0,0,0.85);
  flex-direction: column;
  padding: 10px; gap: 10px;
  animation: fadeInLeft 1s;
  overflow-y: auto;
  z-index: 20;
}
#sidebar button {
  width: 100%;
  text-align: left;
  background: transparent;
  border: 1px solid #00ffff;
  color: #00ffff;
}
#sidebar button:hover {
  background: #00ffff;
  color: #000;
}

.menu-toggle {
  display: none;
  position: fixed;
  top: 15px; left: 15px;
  background: #00ffff;
  color: #000;
  padding: 10px;
  border-radius: 8px;
  cursor: pointer;
  z-index: 40;
  font-weight: bold;
}
.menu-toggle:hover {
  background: #ff00ff;
  color: #fff;
}

#welcomeBox {
  display: none;
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  flex-direction: row;
  gap: 20px;
  animation: fadeInDown 1s;
  z-index: 10;
  flex-wrap: wrap;
  justify-content: center;
}
.neon-card {
  background: rgba(0,255,255,0.1);
  border: 2px solid #00ffff;
  padding: 15px 25px;
  border-radius: 15px;
  text-align: center;
  width: 150px;
  animation: neonPulse 2s infinite alternate;
  box-shadow: 0 0 10px #00ffff, 0 0 20px #00ffff, 0 0 30px #ff00ff;
}
.neon-card h3 {
  font-size: 16px;
  color: #00ffff;
  text-shadow: 0 0 5px #00ffff, 0 0 10px #00ffff, 0 0 20px #ff00ff;
}
.neon-card p {
  font-size: 18px;
  font-weight: bold;
  color: #fff;
  text-shadow: 0 0 5px #00ffff, 0 0 10px #00ffff, 0 0 20px #ff00ff;
}
@keyframes neonPulse {
  0% { box-shadow: 0 0 10px #00ffff, 0 0 20px #00ffff, 0 0 30px #ff00ff; }
  50% { box-shadow: 0 0 20px #00ffff, 0 0 40px #00ffff, 0 0 60px #ff00ff; }
  100% { box-shadow: 0 0 10px #00ffff, 0 0 20px #00ffff, 0 0 30px #ff00ff; }
}

#contentSection {
  margin-left: 270px;
  margin-top: 140px;
  padding: 20px;
  animation: fadeInUp 1s;
}

#profileModal {
  display: none;
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: rgba(0,0,0,0.95);
  justify-content: center;
  align-items: center;
  animation: fadeIn 0.5s;
  z-index: 30;
}
#profileModal div {
  background: #111;
  padding: 20px;
  border-radius: 15px;
  text-align: center;
}

.plan-card {
  background: #111;
  margin: 10px;
  padding: 15px;
  border-radius: 12px;
  border: 1px solid #00ffff;
  transition: 0.3s;
  text-align: center;
}
.plan-card:hover {
  transform: scale(1.05);
  background: #000;
  border-color: #ff00ff;
}

.neon-text {
  color: #00ffff;
  text-shadow: 0 0 5px #00ffff, 0 0 10px #00ffff, 0 0 20px #00ffff, 0 0 40px #ff00ff, 0 0 80px #ff00ff;
}

@keyframes fadeIn { from {opacity:0;} to {opacity:1;} }
@keyframes fadeInLeft { from {opacity:0; transform: translateX(-50px);} to {opacity:1; transform: translateX(0);} }
@keyframes fadeInDown { from {opacity:0; transform: translateY(-50px);} to {opacity:1; transform: translateY(0);} }
@keyframes fadeInUp { from {opacity:0; transform: translateY(50px);} to {opacity:1; transform: translateY(0);} }

@media (max-width: 768px) {
  #sidebar {width: 200px; left: -210px; transition: 0.3s;}
  #sidebar.show {left: 0;}
  #contentSection {margin-left: 0; margin-top: 220px;}
  #welcomeBox {top: 10px; flex-direction: column; gap: 10px;}
  .neon-card {width: 120px; padding: 10px;}
  input {width: 180px;}
}

</style>
</head>

<body>
  <div id="notif"></div>
  <div class="menu-toggle" onclick="toggleSidebar()"><i class="fas fa-bars"></i> Menu</div>

  <div id="authBox">
    <h1 class="text-3xl font-bold mb-4 neon-text">Rock Earn</h1>
    <input type="text" id="authName" placeholder="Your Name (Signup only)">
    <input type="email" id="authEmail" placeholder="Email">
    <input type="password" id="authPass" placeholder="Password">
    <div>
      <button onclick="signupUser()">Sign Up</button>
      <button onclick="loginUser()">Login</button>
    </div>
  </div>

  <div id="sidebar">
    <button onclick="openSection('plans')"><i class="fas fa-list"></i> Plans</button>
    <button onclick="openSection('deposit')"><i class="fas fa-wallet"></i> Deposit</button>
    <button onclick="openSection('withdraw')"><i class="fas fa-money-bill-wave"></i> Withdraw</button>
    <button onclick="openSection('profit')"><i class="fas fa-chart-line"></i> Profit</button>
    <button onclick="openSection('history')"><i class="fas fa-history"></i> History</button>
    <button id="adminBtn" style="display:none;" onclick="openSection('admin')"><i class="fas fa-user-shield"></i> Admin</button>
    <button onclick="openSection('about')"><i class="fas fa-info-circle"></i> About</button>
    <button onclick="openProfile()"><i class="fas fa-user"></i> Profile</button>
    <button onclick="logoutUser()"><i class="fas fa-sign-out-alt"></i> Logout</button>
  </div>

  <div id="welcomeBox">
    <div class="neon-card"><h3>Balance</h3><p id="balValue">0 PKR</p></div>
    <div class="neon-card"><h3>Profit</h3><p id="profValue">0 PKR</p></div>
  </div>

  <div id="contentSection"></div>

  <div id="profileModal">
    <div>
      <h3 class="neon-text">Edit Profile</h3>
      <input type="text" id="profileName" placeholder="Name"><br>
      <input type="email" id="profileEmail" placeholder="Email"><br>
      <input type="password" id="profilePass" placeholder="New Password"><br>
      <button onclick="updateProfile()">Update</button>
      <button onclick="closeProfile()">Close</button>
    </div>
  </div>

<script>
// --- Data Handling ---
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;
if(!users.find(u => u.email === 'admin@rockearn.com')){
  users.push({ name:'John Wilson', email:'admin@rockearn.com', pass:'admin123', role:'admin', deposits:[], withdrawals:[], balance:0, profit:0 });
  localStorage.setItem('reUsers', JSON.stringify(users));
}

// Some plans, more can be added
const plans = [
  {name:'Basic', amount:200, daily:30, days:20},
  {name:'Starter', amount:500, daily:75, days:20},
  {name:'Pro', amount:1000, daily:180, days:20},
  {name:'Advanced', amount:1500, daily:250, days:25},
  {name:'Silver', amount:2000, daily:350, days:30},
  {name:'Gold', amount:3000, daily:550, days:30},
  {name:'Legend', amount:20000, daily:4000, days:80),
  {name:'Titan', amount:30000, daily:6000, days:100, coming: true}
];

// --- Notifications ---
function showNotif(msg) {
  const el = document.getElementById('notif');
  el.innerText = msg;
  el.classList.add('show');
  setTimeout(() => el.classList.remove('show'), 3000);
}

// --- Auth ---
function signupUser(){
  const n = document.getElementById('authName').value.trim();
  const e = document.getElementById('authEmail').value.trim().toLowerCase();
  const p = document.getElementById('authPass').value;
  if(!n || !e || !p){ showNotif('Fill all fields'); return; }
  if(users.find(u => u.email === e)){ showNotif('Email already exists'); return; }
  const u = { name: n, email: e, pass: p, role: 'user', deposits: [], withdrawals: [], balance:0, profit:0 };
  users.push(u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  currentUser = u;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
  showNotif('Account created & logged in');
}

function loginUser(){
  const e = document.getElementById('authEmail').value.trim().toLowerCase();
  const p = document.getElementById('authPass').value;
  const u = users.find(u => u.email === e && u.pass === p);
  if(!u){ showNotif('Invalid credentials'); return; }
  currentUser = u;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
  showNotif('Welcome ' + currentUser.name.split(' ')[0]);
}

function logoutUser(){
  currentUser = null;
  localStorage.removeItem('reCurrent');
  location.reload();
}

// --- Dashboard & Sections ---
function openDashboard(){
  document.getElementById('authBox').style.display = 'none';
  document.getElementById('sidebar').style.display = 'flex';
  document.getElementById('welcomeBox').style.display = 'flex';
  document.getElementById('balValue').innerText = (currentUser.balance || 0) + ' PKR';
  document.getElementById('profValue').innerText = (currentUser.profit || 0) + ' PKR';
  if(currentUser.role === 'admin') {
    document.getElementById('adminBtn').style.display = 'block';
  }
  document.getElementById('contentSection').style.display = 'block';
  openSection('plans');
}

function openSection(type){
  const content = document.getElementById('contentSection');
  content.innerHTML = '';
  if(type === 'plans'){
    plans.forEach((p, idx) => {
      if(p.coming){
        content.innerHTML += `<div class="plan-card"><b>${p.name}</b><br><i>Coming Soon</i></div>`;
      } else {
        content.innerHTML += `<div class="plan-card"><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days})">Buy Now</button></div>`;
      }
    });
  } else if(type === 'deposit'){
    openDeposit();
  } else if(type === 'withdraw'){
    openWithdraw();
  } else if(type === 'history'){
    let html = `<h3>History</h3>`;
    (currentUser.deposits||[]).forEach(d => { html += `<p>Deposit - ${d.plan}: ${d.amount} PKR (TXN: ${d.txn})</p>`; });
    (currentUser.withdrawals||[]).forEach(w => { html += `<p>Withdraw - ${w.amount} PKR via ${w.method}</p>`; });
    content.innerHTML = html;
  } else if(type === 'admin' && currentUser.role === 'admin'){
    let html = `<h3>Admin — Users</h3>`;
    users.forEach(u => {
      html += `<div>${u.name} (${u.email}) — Balance: ${u.balance} PKR, Profit: ${u.profit} PKR</div>`;
    });
    content.innerHTML = html;
  } else if(type === 'about'){
    content.innerHTML = `<h2 class="neon-text">About Rock Earn</h2><p>Owner: John Wilson</p><p>Founded: 2025, California</p><p>Crypto & Binance integration coming soon</p>`;
  }
}

// --- Deposit / Withdraw ---
function copyText(t){
  navigator.clipboard.writeText(t);
  showNotif('Copied: ' + t);
}

function openDeposit(amount=0, name='', daily=0, days=0){
  const content = document.getElementById('contentSection');
  content.innerHTML = `
    <h3>Deposit — ${name}</h3>
    <p>Amount: <strong>${amount} PKR</strong></p>
    <p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
    <p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
    <div style="margin-top:10px">
      <input id="depositTxn" placeholder="Transaction ID"><br>
      <input id="depositProof" type="file"><br>
      <button onclick="confirmDeposit(${amount}, '${name}', ${daily}, ${days})">Submit Deposit</button>
    </div>`;
}

function confirmDeposit(amount, name, daily, days){
  const txnEl = document.getElementById('depositTxn');
  const proofEl = document.getElementById('depositProof');
  const txn = txnEl.value.trim();
  const proof = proofEl.files[0];
  if(!txn || !proof){
    showNotif('Enter transaction ID & upload proof');
    return;
  }
  const deposit = { plan:name, amount, daily, days, txn, proofName: proof.name, ts: Date.now() };
  currentUser.deposits = currentUser.deposits || [];
  currentUser.deposits.push(deposit);
  currentUser.balance = (currentUser.balance || 0) + amount;
  currentUser.profit = (currentUser.profit || 0) + daily;
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  showNotif('Deposit submitted!');
  openSection('plans');
}

function openWithdraw(){
  const content = document.getElementById('contentSection');
  content.innerHTML = `
    <h3>Withdraw</h3>
    <input id="withdrawName" placeholder="Your Name"><br>
    <input id="withdrawAcc" placeholder="Account Number"><br>
    <input id="withdrawAmount" placeholder="Amount"><br>
    <select id="withdrawMethod">
      <option value="JazzCash">JazzCash</option>
      <option value="EasyPaisa">EasyPaisa</option>
      <option value="Bank">Bank</option>
    </select><br><br>
    <button onclick="confirmWithdraw()">Submit Withdraw</button>`;
}

function confirmWithdraw(){
  const name = document.getElementById('withdrawName').value.trim();
  const acc = document.getElementById('withdrawAcc').value.trim();
  const amt = parseFloat(document.getElementById('withdrawAmount').value);
  const method = document.getElementById('withdrawMethod').value;
  if(!name || !acc || !amt || amt > (currentUser.balance || 0)){
    showNotif('Invalid withdraw request');
    return;
  }
  const wd = { name, account: acc, amount: amt, method, ts: Date.now() };
  currentUser.withdrawals = currentUser.withdrawals || [];
  currentUser.withdrawals.push(wd);
  currentUser.balance -= amt;
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  showNotif('Withdraw request submitted');
  openSection('history');
}

// --- Profile ---
function openProfile(){
  document.getElementById('profileModal').style.display = 'flex';
  document.getElementById('profileName').value = currentUser.name;
  document.getElementById('profileEmail').value = currentUser.email;
  document.getElementById('profilePass').value = '';
}
function closeProfile(){
  document.getElementById('profileModal').style.display = 'none';
}
function updateProfile(){
  const n = document.getElementById('profileName').value.trim();
  const e = document.getElementById('profileEmail').value.trim().toLowerCase();
  const p = document.getElementById('profilePass').value;
  if(!n || !e){
    showNotif('Name & Email required');
    return;
  }
  if(users.some(u => u.email === e && u.email !== currentUser.email)){
    showNotif('Email already in use');
    return;
  }
  currentUser.name = n;
  currentUser.email = e;
  if(p) currentUser.pass = p;
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  showNotif('Profile updated');
  closeProfile();
  openDashboard();
}

// --- Init ---
if(currentUser) {
  openDashboard();
}
</script>

</body>
</html>
