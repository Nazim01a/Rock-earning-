<ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Full Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body{
  font-family:Arial,sans-serif;
  margin:0;
  padding:20px;
  color:#fff;
  background:linear-gradient(120deg,#0a0227,#24006e,#4e0bb3);
  background-size:400% 400%;
  animation:bgAnim 12s ease infinite;
}
@keyframes bgAnim{
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}
button{padding:8px 12px;margin:4px;cursor:pointer}
#sidebar{display:none;margin-top:20px}
.icon-btn{display:inline-block;width:100px;height:80px;text-align:center;border:1px solid #000;border-radius:8px;padding:8px;margin:4px;cursor:pointer}
#contentSection{margin-top:20px}
.plan-card{border:1px solid #000;padding:10px;margin:10px 0}
</style>
</head>
<body><h2>Rock Earn Dashboard</h2>
<div id="authBox">
<h3>Login / Sign Up / Forgot Password</h3>
<input id="authName" placeholder="Full Name"><br>
<input id="authEmail" placeholder="Email"><br>
<input id="authPass" type="password" placeholder="Password"><br>
<button id="signupBtn">Sign Up</button>
<button id="loginBtn">Login</button>
<button id="forgotBtn">Forgot Password</button>
</div><div id="notif"></div><div id="sidebar">
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><br>Plans</div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><br>Deposit</div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><br>Withdraw</div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><br>Profit</div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><br>History</div>
<div class="icon-btn" onclick="openSection('admin')" id="adminBtn" style="display:none"><i class="fa fa-cog"></i><br>Admin</div>
</div><div id="contentSection"></div><script>
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

// Ensure default admin
if(!users.find(u => u.email==='admin@rockearn.com')){
  users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
  localStorage.setItem('reUsers',JSON.stringify(users));
}

const plans = [
  {name:'Plan 1',amount:200,daily:30,days:20},
  {name:'Plan 2',amount:300,daily:45,days:20},
  {name:'Plan 3',amount:400,daily:60,days:20},
  {name:'Plan 4',amount:500,daily:75,days:20},
  {name:'Plan 5',amount:650,daily:95,days:25},
  {name:'Plan 6',amount:800,daily:120,days:25},
  {name:'Plan 7',amount:1000,daily:150,days:30},
  {name:'Plan 8',amount:1500,daily:230,days:30},
  {name:'Plan 9',amount:2000,daily:320,days:30},
  {name:'Plan 10',amount:2500,daily:400,days:35},
  {name:'Plan 11',amount:3000,daily:480,days:35},
  {name:'Plan 12',amount:4000,daily:650,days:40},
  {name:'Plan 13',amount:5000,daily:820,days:40},
  {name:'Plan 14',amount:8000,daily:1320,days:45},
  {name:'Plan 15',amount:15000,daily:2500,days:50}
];

function showNotif(msg){document.getElementById('notif').innerText=msg;setTimeout(()=>{document.getElementById('notif').innerText=''},3000)}

function signupUser(){
  const n=document.getElementById('authName').value.trim();
  const e=document.getElementById('authEmail').value.trim().toLowerCase();
  const p=document.getElementById('authPass').value.trim();

  if(!n||!e||!p){showNotif('Fill all fields');return;}

  users = JSON.parse(localStorage.getItem('reUsers')) || [];

  if(users.find(u=>u.email===e)){showNotif('Email already exists');return;}

  let newUser={name:n,email:e,pass:p,role:'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};

  users.push(newUser);
  localStorage.setItem('reUsers',JSON.stringify(users));

  currentUser=newUser;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));

  showDashboard();
  showNotif('Account created successfully');('Fill all fields');return;}
  if(users.find(u=>u.email===e)){showNotif('Email exists');return;}
  let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
  users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  document.getElementById('authBox').style.display='none';
  document.getElementById('sidebar').style.display='block';
  openDashboard();showNotif('Account created & logged in');
}

function loginUser(){
  const e=document.getElementById('authEmail').value.trim().toLowerCase();
  const p=document.getElementById('authPass').value.trim();

  users = JSON.parse(localStorage.getItem('reUsers')) || [];
  const u=users.find(x=>x.email===e && x.pass===p);

  if(!u){showNotif('Invalid email or password');return;}

  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));

  showDashboard();
  showNotif('Welcome '+u.name);('Invalid credentials');return;}
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  document.getElementById('authBox').style.display='none';
  document.getElementById('sidebar').style.display='block';
  openDashboard();showNotif('Welcome '+currentUser.name.split(' ')[0]);
}

function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

function openDashboard(){showDashboard();}</b> 🎉</h2>`;
  if(currentUser.role==='admin'){
    document.getElementById('adminBtn').style.display='inline-block';
  }
}
}

function openSection(type){
  const content=document.getElementById('contentSection');
  content.innerHTML='';
  if(type==='plans'){ 
    let html = '<h3>Available Plans</h3>'; 
    plans.forEach((p,idx)=>{
      html += `<div class='plan-card'><b>${p.name}</b><div class='small'>${p.days} days • Daily ${p.daily} PKR</div><div>Amount: ${p.amount} PKR</div><div style='margin-top:8px'><button onclick="openDepositForPlan(${idx})">Buy Now</button></div></div>`;
    });
    content.innerHTML = html;
  } 
  else if(type==='deposit'){
    content.innerHTML=`<h3>Deposit</h3>
    <p>Select a plan first from Plans page.</p>
    <p>Recent Deposits:</p>
    ${currentUser.deposits.map(d=>`<div>Method: ${d.method} — Amount: ${d.amount} PKR — Status: ${d.status}</div>`).join('')}`;
  } 
  else if(type==='deposit'){{content.innerHTML='<h3>Deposit Section</h3>'} 
  else if(type==='withdraw'){
    content.innerHTML=`<h3>Withdraw</h3>
    <p>Balance: ${currentUser.balance} PKR</p>
    <input id='wdAmount' placeholder='Amount'><br>
    <select id='wdMethod'>
      <option value='JazzCash'>JazzCash</option>
      <option value='EasyPaisa'>EasyPaisa</option>
      <option value='Bank'>Bank Transfer</option>
    </select><br>
    <button onclick='submitWithdraw()'>Submit Request</button>`;
  }
  else if(type==='profit'){
    let html = `<h3>Your Profit</h3>`;
    const plans = (currentUser.plans||[]);
    if(plans.length===0) html += '<div class="small">No active plans</div>';
    else{
      let totalDaily = 0;
      plans.forEach(p=>{ totalDaily += (p.daily||0); html += `<div class='plan-card'><b>${p.name}</b> — Daily: <b>${p.daily} PKR</b> — Started: ${new Date(p.start).toLocaleString()}</div>`; });
      html += `<div style="margin-top:8px"><b>Total daily profit:</b> ${totalDaily} PKR</div>`;
    }
    content.innerHTML = html;
  }
  else if(type==='history'){
    content.innerHTML=`<h3>History</h3>
    <b>Plans:</b><br>${currentUser.plans.map(p=>`<div>${p.name} — ${p.amount} PKR</div>`).join('')}<br><br>
    <b>Deposits:</b><br>${currentUser.deposits.map(d=>`<div>${d.method} — ${d.amount} PKR — ${d.status}</div>`).join('')}<br><br>
    <b>Withdrawals:</b><br>${currentUser.withdrawals.map(w=>`<div>${w.method} — ${w.amount} PKR — ${w.status}</div>`).join('')}`;
  }
  else if(type==='admin' && currentUser.role==='admin'){
    let html='<h3>Admin Panel</h3>';
    users.forEach(u=>{
      html+=`<div style='border:1px solid #000;margin:5px;padding:5px'>
      <b>${u.name}</b> (${u.email})<br>
      Balance: ${u.balance} PKR<br>
      <b>Deposits:</b><br>
      ${u.deposits.map((d,i)=>`<div>Amount: ${d.amount} — ${d.method} — Status: ${d.status}
      ${d.status==='Pending'?`<button onclick="approveDeposit('${u.email}',${i})">Approve</button>`:''}</div>`).join('')}<br>
      <b>Withdrawals:</b><br>
      ${u.withdrawals.map((w,i)=>`<div>Amount: ${w.amount} — ${w.method} — Status: ${w.status}
      ${w.status==='Pending'?`<button onclick="approveWithdraw('${u.email}',${i})">Approve</button>`:''}</div>`).join('')}
      </div>`;
    });
    content.innerHTML=html;`});
    content.innerHTML=html;
  } else {content.innerHTML='<h3>Coming Soon</h3>'}
}

function buyPlan(name,amount,daily,days){
  // Open deposit form instead of instantly buying
  openSection('deposit');
  const content=document.getElementById('contentSection');
  content.innerHTML=`<h3>Deposit to Activate Plan</h3>
  <p><b>Selected Plan:</b> ${name}</p>
  <p><b>Required Amount:</b> ${amount} PKR</p>
  <p>Choose Method:</p>
  <select id='depMethod'>
    <option value='JazzCash'>JazzCash</option>
    <option value='EasyPaisa'>EasyPaisa</option>
  </select><br><br>
  <button onclick="confirmDeposit('${name}',${amount},${daily},${days})">Confirm Deposit</button>`;
}

function confirmDeposit(name,amount,daily,days){
  const method=document.getElementById('depMethod').value;
  currentUser.deposits.push({method,amount,time:Date.now(),status:'Pending'});
  currentUser.plans.push({name,amount,daily,days,start:Date.now()});
  users=users.map(u=>u.email===currentUser.email?currentUser:u);
  localStorage.setItem('reUsers',JSON.stringify(users));
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  showNotif(`Deposit submitted & Plan ${name} activated`);
}(`Plan ${name} bought successfully`);
}

document.getElementById('signupBtn').onclick=signupUser;
document.getElementById('loginBtn').onclick=loginUser;
function submitWithdraw(){
  const amt=document.getElementById('wdAmount').value;
  const method=document.getElementById('wdMethod').value;
  if(!amt){showNotif('Enter amount');return;}
  currentUser.withdrawals.push({amount:amt,method,status:'Pending',time:Date.now()});
  users=users.map(u=>u.email===currentUser.email?currentUser:u);
  localStorage.setItem('reUsers',JSON.stringify(users));
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  showNotif('Withdrawal request submitted');
}

function approveDeposit(email,index){
  let u=users.find(x=>x.email===email);
  u.deposits[index].status='Approved';
  u.balance+=u.deposits[index].amount;
  users=users.map(x=>x.email===email?u:x);
  localStorage.setItem('reUsers',JSON.stringify(users));
  showNotif('Deposit Approved');
  openSection('admin');
}

function approveWithdraw(email,index){
  let u=users.find(x=>x.email===email);
  u.withdrawals[index].status='Approved';
  u.balance-=u.withdrawals[index].amount;
  users=users.map(x=>x.email===email?u:x);
  localStorage.setItem('reUsers',JSON.stringify(users));
  showNotif('Withdrawal Approved');
  openSection('admin');
}

function showDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('sidebar').style.display='block';
  if(currentUser.role==='admin'){
    document.getElementById('adminBtn').style.display='inline-block';
  }
  document.getElementById('contentSection').innerHTML='<h3>Welcome '+currentUser.name+'</h3>';
}

</script></body>
</html>
