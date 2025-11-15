<80>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Premium</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"></script>
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;color:white;overflow-x:hidden;background:linear-gradient(135deg,#111,#222);}
body::before{content:"";position:fixed;top:0;left:0;right:0;bottom:0;z-index:-1;background:linear-gradient(270deg,#ff00ff,#00ffff,#ff9900,#ff00ff);background-size:800% 800%;animation:bgAnim 20s ease infinite;}
@keyframes bgAnim{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#notif{position:fixed;top:10px;left:50%;transform:translateX(-50%);background:#00ff99;padding:10px 20px;border-radius:8px;display:none;z-index:9999;font-weight:bold;}
#notif.show{display:block;animation:fadeInOut 3s forwards;}
@keyframes fadeInOut{0%{opacity:0}10%{opacity:1}90%{opacity:1}100%{opacity:0}}
#rockEarnTitle{font-size:3rem;font-weight:bold;text-align:center;margin:20px 0;background:linear-gradient(90deg,#ff00ff,#00ffff,#ff9900,#ff00ff);background-size:300% 300%;-webkit-background-clip:text;-webkit-text-fill-color:transparent;animation:gradientAnim 6s ease infinite;}
@keyframes gradientAnim{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#sidebar{display:flex;flex-direction:column;position:fixed;left:0;top:0;bottom:0;width:80px;background:#111;padding-top:60px;z-index:10;}
#sidebar button{margin:10px 0;background:none;border:none;color:white;font-size:1.2rem;cursor:pointer;transition:all 0.3s;}
#sidebar button:hover{color:#00ffcc;transform:scale(1.2);}
#adminBtn{display:none;background:#ff9900;color:black;font-weight:bold;padding:8px 12px;border-radius:6px;margin-top:10px;}
#adminBtn:hover{background:#00ffff;color:black;transform:scale(1.1);}
#contentSection{margin-left:100px;padding:20px;animation:fadeContent 0.5s;}
@keyframes fadeContent{0%{opacity:0}100%{opacity:1}}
.plan-card{background:rgba(255,255,255,0.05);border:1px solid #00ffcc;padding:15px;margin:10px;border-radius:12px;display:inline-block;width:200px;transition:transform 0.3s,box-shadow 0.3s;}
.plan-card:hover{transform:translateY(-5px);box-shadow:0 0 15px #00ffcc;}
.plan-card button{background:#00ffcc;color:#000;border:none;padding:8px 12px;border-radius:6px;cursor:pointer;font-weight:bold;transition:all 0.3s;}
.plan-card button:hover{background:#ff00ff;color:white;transform:scale(1.1);}
#welcomeBox{text-align:center;padding:10px;margin-top:60px;}
#welcomeBox div{display:inline-block;background:rgba(0,255,204,0.1);padding:10px 20px;margin:5px;border-radius:10px;animation:popIn 0.5s;}
@keyframes popIn{0%{transform:scale(0)}100%{transform:scale(1)}}
#backBtn,#refreshBtn{position:fixed;top:15px;cursor:pointer;z-index:10;padding:8px 12px;border:none;border-radius:6px;font-weight:bold;background:#00ffcc;color:#000;transition:all 0.3s;}
#backBtn:hover,#refreshBtn:hover{background:#ff00ff;color:white;transform:scale(1.1);}
#backBtn{left:100px;} #refreshBtn{right:20px;}
</style>
</head>
<body>
<div id="notif"></div>
<div id="rockEarnTitle">Rock Earn</div>
<div id="authBox" style="text-align:center;margin-top:50px;">
<input id="authName" placeholder="Name"><br><br>
<input id="authEmail" placeholder="Email"><br><br>
<input id="authPass" type="password" placeholder="Password"><br><br>
<button onclick="signupUser()">Sign Up</button>
<button onclick="loginUser()">Login</button>
</div>
<div id="sidebar" style="display:none;">
<button onclick="openSection('plans')"><i class="fa-solid fa-list"></i></button>
<button onclick="openSection('deposit')"><i class="fa-solid fa-wallet"></i></button>
<button onclick="openSection('withdraw')"><i class="fa-solid fa-arrow-up-right-from-square"></i></button>
<button onclick="openSection('profit')"><i class="fa-solid fa-money-bill-trend-up"></i></button>
<button onclick="openSection('history')"><i class="fa-solid fa-clock-rotate-left"></i></button>
<button id="adminBtn" onclick="openSection('admin')"><i class="fa-solid fa-user-gear"></i></button>
</div>
<div id="welcomeBox" style="display:none;">
<div id="userNameTxt"></div>
<div>Balance: <span id="balValue">0</span></div>
<div>Profit: <span id="profValue">0</span></div>
</div>
<div id="contentSection" style="display:none;">
<button id="backBtn" onclick="closeSection()">Back</button>
<button id="refreshBtn" onclick="refreshDashboard()">Refresh</button>
<div id="sectionContent"></div>
</div>
<div id="profileModal" style="display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#111;padding:20px;border-radius:10px;z-index:20;">
<h3>Edit Profile</h3>
<input id="profileName" placeholder="Name"><br><br>
<input id="profileEmail" placeholder="Email"><br><br>
<input id="profilePass" type="password" placeholder="New Password"><br><br>
<button onclick="updateProfile()">Update</button>
<button onclick="closeProfile()">Close</button>
</div>
<script>// ----------------------
// Part 2 — Full JS Functionality
// Paste this inside the <script> block after the Part 1 HTML
// ----------------------

/* Users & Storage */
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

/* Ensure default admin exists */
if(!users.find(u => u.email === 'admin@rockearn.com')){
  users.push({
    name: 'Administrator',
    email: 'admin@rockearn.com',
    pass: 'admin123',
    role: 'admin',
    plans: [],
    deposits: [],
    withdrawals: [],
    balance: 0,
    profit: 0
  });
  localStorage.setItem('reUsers', JSON.stringify(users));
}

/* Plans array (15 plans) */
const plans = [
  {name:'Basic',amount:200,daily:30,days:20},
  {name:'Starter',amount:500,daily:75,days:20},
  {name:'Pro',amount:1000,daily:180,days:20},
  {name:'Advanced',amount:1500,daily:250,days:25},
  {name:'Silver',amount:2000,daily:350,days:30},
  {name:'Gold',amount:3000,daily:550,days:30},
  {name:'Platinum',amount:5000,daily:950,days:40},
  {name:'Diamond',amount:7000,daily:1350,days:50},
  {name:'VIP',amount:10000,daily:2000,days:60},
  {name:'Elite',amount:12000,daily:2400,days:60},
  {name:'Master',amount:15000,daily:3000,days:70},
  {name:'Ultimate',amount:18000,daily:3500,days:75},
  {name:'Legend',amount:20000,daily:4000,days:80},
  {name:'Supreme',amount:25000,daily:5000,days:90},
  {name:'Titan',amount:30000,daily:6000,days:100}
];

/* Helper: show notification */
function showNotif(msg){
  const el = document.getElementById('notif');
  if(!el) return;
  el.innerText = msg;
  el.classList.add('show');
  // fallback display if CSS .show is not used
  el.style.display = 'block';
  setTimeout(()=>{
    el.classList.remove('show');
    el.style.display = 'none';
  }, 3000);
}

/* AUTH: signup */
function signupUser(){
  const n = document.getElementById('authName').value.trim();
  const e = document.getElementById('authEmail').value.trim().toLowerCase();
  const p = document.getElementById('authPass').value;
  if(!n || !e || !p){ showNotif('Fill all fields'); return; }
  if(users.find(u => u.email === e)){ showNotif('Email exists'); return; }

  const u = {
    name: n,
    email: e,
    pass: p,
    role: e === 'admin@rockearn.com' ? 'admin' : 'user',
    plans: [],
    deposits: [],
    withdrawals: [],
    balance: 0,
    profit: 0
  };
  users.push(u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  currentUser = u;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
  showNotif('Account created & logged in');
}

/* AUTH: login */
function loginUser(){
  const e = document.getElementById('authEmail').value.trim().toLowerCase();
  const p = document.getElementById('authPass').value;
  const u = users.find(x => x.email === e && x.pass === p);
  if(!u){ showNotif('Invalid credentials'); return; }
  currentUser = u;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
  showNotif('Welcome ' + currentUser.name.split(' ')[0]);
}

/* LOGOUT */
function logoutUser(){
  currentUser = null;
  localStorage.removeItem('reCurrent');
  location.reload();
}

/* OPEN DASHBOARD */
function openDashboard(){
  document.getElementById('authBox').style.display = 'none';
  document.getElementById('sidebar').style.display = 'flex';
  document.getElementById('contentSection').style.display = 'block';
  document.getElementById('welcomeBox').style.display = 'block';
  document.getElementById('refreshBtn').style.display = 'inline-block';
  // set user info text (if elements exist)
  const nameEl = document.getElementById('userNameTxt');
  if(nameEl) nameEl.innerText = currentUser.name.split(' ')[0];
  const balEl = document.getElementById('balValue');
  if(balEl) balEl.innerText = (currentUser.balance || 0) + ' PKR';
  const profEl = document.getElementById('profValue');
  if(profEl) profEl.innerText = (currentUser.profit || 0) + ' PKR';
  // show admin button if admin
  if(currentUser.role === 'admin'){
    const adminBtn = document.getElementById('adminBtn');
    if(adminBtn) adminBtn.style.display = 'inline-block';
  }
  // hide back initially
  const back = document.getElementById('backBtn');
  if(back) back.style.display = 'none';
}

/* REFRESH DASHBOARD — prevents auto-logout */
function refreshDashboard(){
  if(!currentUser) { showNotif('No user logged in'); return; }
  // reload fresh user state from storage (in case another tab changed)
  users = JSON.parse(localStorage.getItem('reUsers')) || users;
  currentUser = users.find(u => u.email === currentUser.email) || currentUser;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  const balEl = document.getElementById('balValue');
  if(balEl) balEl.innerText = (currentUser.balance || 0) + ' PKR';
  const profEl = document.getElementById('profValue');
  if(profEl) profEl.innerText = (currentUser.profit || 0) + ' PKR';
  showNotif('Dashboard refreshed');
  closeSection();
}

/* OPEN SECTION (plans, deposit, withdraw, profit, history, admin, profile) */
function openSection(type){
  const content = document.getElementById('sectionContent');
  if(!content) return;
  document.getElementById('backBtn').style.display = 'inline-block';
  content.innerHTML = '';

  if(type === 'plans'){
    // render plan cards
    plans.forEach((p, idx) => {
      const card = document.createElement('div');
      card.className = 'plan-card';
      card.innerHTML = `<b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><br>`;
      const btn = document.createElement('button');
      btn.innerText = 'Buy Now';
      btn.onclick = () => openDeposit(p.amount, p.name, p.daily, p.days, idx);
      card.appendChild(btn);
      content.appendChild(card);
    });
  }
  else if(type === 'deposit'){
    openDeposit(); // generic deposit section
  }
  else if(type === 'withdraw'){
    openWithdraw();
  }
  else if(type === 'profit'){
    content.innerHTML = `<h3>Total Profit</h3><p>${(currentUser.profit||0)} PKR</p>`;
  }
  else if(type === 'history'){
    let html = '<h3>History</h3>';
    (currentUser.deposits || []).forEach(d => {
      html += `<p>Deposit: ${d.plan} - ${d.amount} PKR - ${d.status}</p>`;
    });
    (currentUser.withdrawals || []).forEach(w => {
      html += `<p>Withdraw: ${w.amount} PKR - ${w.method} - ${w.status}</p>`;
    });
    content.innerHTML = html;
  }
  else if(type === 'admin' && currentUser.role === 'admin'){
    // Admin: show list of users and activity (deposits/withdrawals)
    let html = '<h3>Admin — User Activity</h3>';
    users.forEach(u => {
      html += `<div style="padding:10px;border-radius:8px;margin-bottom:8px;background:rgba(255,255,255,0.02)">
        <strong>${u.name}</strong> (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR
        <div style="font-size:13px;margin-top:6px;"><b>Deposits:</b>`;
      (u.deposits || []).forEach(d => html += `<div style="margin-left:6px">• ${d.plan} — ${d.amount} PKR — ${d.status}</div>`);
      html += `</div><div style="font-size:13px;margin-top:6px;"><b>Withdrawals:</b>`;
      (u.withdrawals || []).forEach(w => html += `<div style="margin-left:6px">• ${w.amount} PKR — ${w.method} — ${w.status}</div>`);
      html += `</div></div>`;
    });
    content.innerHTML = html;
  }
  else if(type === 'profile'){
    openProfile();
  }
  else {
    content.innerHTML = '<h3>Coming Soon</h3>';
  }
}

/* Close section (hide back button) */
function closeSection(){
  const back = document.getElementById('backBtn');
  if(back) back.style.display = 'none';
  // keep contentSection visible; we don't auto-hide the whole area so UI remains stable
}

/* OPEN DEPOSIT — prefilled when called from Buy Now */
function openDeposit(amount=0, name='', daily=0, days=0, planIndex=0){
  const content = document.getElementById('sectionContent');
  if(!content) return;
  document.getElementById('backBtn').style.display = 'inline-block';
  content.innerHTML = `
    <h3>Deposit — ${ name || 'Manual' }</h3>
    <p>Amount: <strong>${amount} PKR</strong></p>
    <p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
    <p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
    <p>Bank: <em>Coming Soon</em></p>
    <p>Binance: <em>Coming Soon</em></p>
    <div style="margin-top:10px">
      <input id="depositTxn" placeholder="Transaction ID" style="padding:8px;width:60%;margin-right:6px;">
      <input id="depositProof" type="file" style="padding:8px;"><br><br>
      <button onclick="confirmDeposit(${amount},'${escapeHtml(name)}',${daily},${days},${planIndex})">Submit Deposit</button>
    </div>
  `;
}

/* CONFIRM DEPOSIT — stores deposit, updates balance and gives first-day profit */
function confirmDeposit(amount, name, daily, days, planIndex){
  const txn = document.getElementById('depositTxn')?.value.trim();
  const proofInput = document.getElementById('depositProof');
  const proof = proofInput && proofInput.files && proofInput.files[0];
  if(!txn || !proof){ showNotif('Upload proof & enter txn'); return; }

  // Save deposit entry (we store only file name, not binary)
  const deposit = {
    plan: name,
    amount: amount,
    daily: daily,
    days: days,
    txn: txn,
    proof: proof.name || 'proof',
    status: 'approved', // auto-approved as requested
    ts: Date.now()
  };

  currentUser.deposits = currentUser.deposits || [];
  currentUser.deposits.push(deposit);

  // Update balance and add first day profit immediately
  currentUser.balance = (currentUser.balance || 0) + Number(amount || 0);
  currentUser.profit = (currentUser.profit || 0) + Number(daily || 0);

  // Persist users
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));

  showNotif('Deposit submitted & balance updated');
  openSection('plans');
}

/* OPEN WITHDRAWAL UI */
function openWithdraw(){
  const content = document.getElementById('sectionContent');
  if(!content) return;
  document.getElementById('backBtn').style.display = 'inline-block';
  content.innerHTML = `
    <h3>Withdraw</h3>
    <input id="withdrawName" placeholder="Your Name" style="padding:8px;width:60%;margin-right:6px;"><br><br>
    <input id="withdrawAcc" placeholder="Account Number" style="padding:8px;width:60%;margin-right:6px;"><br><br>
    <input id="withdrawAmount" placeholder="Amount" style="padding:8px;width:60%;margin-right:6px;"><br><br>
    <select id="withdrawMethod" style="padding:8px;">
      <option value="JazzCash">JazzCash</option>
      <option value="EasyPaisa">EasyPaisa</option>
      <option value="Bank">Bank</option>
      <option value="Binance" disabled>Binance (Coming Soon)</option>
    </select><br><br>
    <button onclick="confirmWithdraw()">Submit Withdraw</button>
  `;
}

/* CONFIRM WITHDRAW — deducts balance and logs withdrawal */
function confirmWithdraw(){
  const name = document.getElementById('withdrawName')?.value.trim();
  const acc = document.getElementById('withdrawAcc')?.value.trim();
  const amtRaw = document.getElementById('withdrawAmount')?.value.trim();
  const amt = parseInt(amtRaw, 10);
  const method = document.getElementById('withdrawMethod')?.value;
  if(!name || !acc || !amt || isNaN(amt)){ showNotif('Fill all fields correctly'); return; }
  if(amt > (currentUser.balance || 0)){ showNotif('Insufficient balance'); return; }

  currentUser.balance = (currentUser.balance || 0) - amt;
  currentUser.withdrawals = currentUser.withdrawals || [];
  currentUser.withdrawals.push({
    name: name,
    account: acc,
    amount: amt,
    method: method,
    status: 'done',
    ts: Date.now()
  });

  // Persist
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  showNotif('Withdraw request processed');
  openSection('withdraw');
}

/* COPY helper */
function copyText(txt){
  if(navigator && navigator.clipboard && navigator.clipboard.writeText){
    navigator.clipboard.writeText(txt).then(()=> showNotif('Copied to clipboard'));
  } else {
    showNotif('Copy not supported');
  }
}

/* PROFILE modal functions */
function openProfile(){
  if(!currentUser){ showNotif('No user logged in'); return; }
  document.getElementById('profileModal').style.display = 'block';
  document.getElementById('profileName').value = currentUser.name;
  document.getElementById('profileEmail').value = currentUser.email;
}
function closeProfile(){
  document.getElementById('profileModal').style.display = 'none';
}
function updateProfile(){
  const n = document.getElementById('profileName')?.value.trim();
  const e = document.getElementById('profileEmail')?.value.trim().toLowerCase();
  const p = document.getElementById('profilePass')?.value.trim();
  if(n) currentUser.name = n;
  if(e) currentUser.email = e;
  if(p) currentUser.pass = p;
  // persist changes
  users = users.map(u => u.email === currentUser.email ? currentUser : u);
  // if email changed we need to update storage properly: replace by matching previous email
  localStorage.setItem('reUsers', JSON.stringify(users));
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  showNotif('Profile updated');
  closeProfile();
}

/* Utility: basic html escape for strings used inside template (to avoid breaking attributes) */
function escapeHtml(str){
  if(!str) return '';
  return String(str).replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/'/g,'&#39;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

/* INIT: if currentUser present, open dashboard */
if(currentUser){
  // ensure currentUser is refreshed from users list (in case of updated localStorage)
  users = JSON.parse(localStorage.getItem('reUsers')) || users;
  currentUser = users.find(u => u.email === currentUser.email) || currentUser;
  localStorage.setItem('reCurrent', JSON.stringify(currentUser));
  openDashboard();
}

/* End of Part 2 JS */
</script>
</body>
</html>
