<ROCKS>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>RockEarn — Live</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{
    --bg-1: #071025;
    --bg-2: #0f1724;
    --card: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
    --accent-a: #7c5cff;
    --accent-b: #3ad6c7;
    --muted: #9aa6bf;
    --glass: rgba(255,255,255,0.03);
  }
  body{ background: linear-gradient(180deg,var(--bg-1),var(--bg-2)); color:#e8f0ff; font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto; min-height:100vh; }
  .card{ background:var(--card); border-radius:14px; padding:16px; box-shadow: 0 10px 30px rgba(2,6,23,0.6); }
  .logo{ width:44px;height:44px;border-radius:10px; display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg,var(--accent-a),var(--accent-b)); color:#041226; font-weight:800; }
  .btn-primary{ background: linear-gradient(90deg,var(--accent-b),var(--accent-a)); color:#041226; font-weight:700; padding:10px 14px; border-radius:10px; }
  .btn-ghost{ background:transparent; border:1px solid rgba(255,255,255,0.06); padding:8px 12px; border-radius:10px; color:var(--muted); }
  input, select, textarea{ background: rgba(255,255,255,0.02); border:1px solid rgba(255,255,255,0.03); padding:10px; border-radius:8px; color:inherit; width:100%; }
  .plan-card{ transition: transform .22s, box-shadow .22s; }
  .plan-card:hover{ transform: translateY(-6px); box-shadow: 0 20px 40px rgba(0,0,0,0.6); }
  .small{ font-size:13px; color:var(--muted); }
  .muted{ color:var(--muted); }
  .float-right{ float:right; }
  .hidden{ display:none !important; }
  .mt-2{ margin-top:8px; } .mt-3{ margin-top:12px; } .mt-4{ margin-top:16px; } .mb-2{ margin-bottom:8px; } .mb-3{ margin-bottom:12px; }
  .center{ display:flex; align-items:center; gap:10px; }
  .file-preview{ max-width:220px; border-radius:10px; border:1px solid rgba(255,255,255,0.04); margin-top:8px; display:block; }
  footer{ color:var(--muted); font-size:13px; margin-top:18px; text-align:center; }
  /* responsive */
  @media (max-width:900px){ .grid-3{ grid-template-columns: 1fr !important; } }
</style>
</head>
<body>

<!-- TOP NAV -->
<header class="max-w-6xl mx-auto p-4 flex items-center justify-between">
  <div class="center">
    <div class="logo">RE</div>
    <div>
      <div style="font-weight:700; font-size:18px">RockEarn</div>
      <div class="small muted">Secure • Fast • Local</div>
    </div>
  </div>
  <div class="center">
    <div id="navUser" class="small muted">Not signed in</div>
    <button id="openAuthBtn" class="btn-ghost ml-3">Login / Signup</button>
  </div>
</header>

<!-- MAIN -->
<main class="max-w-6xl mx-auto px-4 pb-10">
  <div class="grid grid-cols-3 gap-6 grid-3">

    <!-- LEFT: Balance + Actions -->
    <section class="card">
      <div class="flex items-start justify-between">
        <div>
          <div class="small muted">Available Balance</div>
          <div id="balance" style="font-size:28px; font-weight:800; margin-top:6px">₨ 0</div>
        </div>
        <div class="text-right">
          <div id="welcomeUser" class="small muted">Guest</div>
          <div class="small muted">Client-side</div>
        </div>
      </div>

      <div class="mt-4 space-y-3">
        <button id="btnDeposit" class="btn-primary w-full">Deposit</button>
        <button id="btnWithdraw" class="btn-ghost w-full">Withdraw</button>
        <button id="btnShare" class="btn-ghost w-full">Share Referral</button>
      </div>

      <div class="mt-4 small muted">
        <div><strong>JazzCash:</strong> <span class="font-mono">03705519562</span></div>
        <div class="mt-2"><strong>EasyPaisa:</strong> <span class="font-mono">03379827882</span></div>
      </div>

      <div class="mt-4 small muted">
        <div><strong>Customer Service:</strong> WhatsApp <span class="font-mono">+92 300 1234567</span></div>
        <div class="mt-2"><strong>Company:</strong> RockEarn Pvt Ltd</div>
      </div>
    </section>

    <!-- MIDDLE: Plans -->
    <section class="card">
      <div class="flex items-center justify-between">
        <h3 style="font-weight:700">Investment Plans</h3>
        <div class="small muted">Choose any plan to deposit</div>
      </div>

      <div id="plansWrap" class="mt-4 grid grid-cols-1 gap-3"></div>
    </section>

    <!-- RIGHT: History + Profile -->
    <section class="card">
      <div class="flex items-center justify-between">
        <h3 style="font-weight:700">Profile & History</h3>
        <div class="small muted">Manage account</div>
      </div>

      <div id="profileArea" class="mt-3">
        <!-- dynamic -->
        <div class="small muted">Not signed in. Please login or signup.</div>
      </div>

      <div class="mt-3">
        <button id="btnProfile" class="btn-ghost w-full">Open Profile</button>
        <button id="btnSupport" class="btn-ghost w-full mt-2">Contact Support</button>
      </div>
    </section>

  </div>
</main>

<footer class="max-w-6xl mx-auto">© RockEarn — For demo & GitHub live preview</footer>

<!-- MODALS / PAGES ROOTS -->
<div id="modalRoot"></div>
<div id="authRoot"></div>

<script>
/* ---------------- State ---------------- */
let users = JSON.parse(localStorage.getItem('re_users') || '[]'); // array of users
let currentUser = JSON.parse(localStorage.getItem('re_current') || 'null'); // single user object or null

// utility functions
const $ = (id)=>document.getElementById(id);
function saveState(){ localStorage.setItem('re_users', JSON.stringify(users)); localStorage.setItem('re_current', JSON.stringify(currentUser)); }
function money(n){ return '₨ ' + Number(n).toLocaleString(); }
function el(html){ const d=document.createElement('div'); d.innerHTML = html.trim(); return d.firstChild; }

/* ---------------- Plans Data ---------------- */
const plans = [
  {id:1, name:'Starter', price:180, daily:'3%', days:7, desc:'Entry level — reliable'},
  {id:2, name:'Bronze', price:500, daily:'3.5%', days:10, desc:'Beginner friendly'},
  {id:3, name:'Silver', price:1000, daily:'4%', days:12, desc:'Balanced returns'},
  {id:4, name:'Gold', price:1500, daily:'4.5%', days:15, desc:'Popular choice'},
  {id:5, name:'Platinum', price:2500, daily:'5%', days:18, desc:'High return tier'},
  {id:6, name:'Diamond', price:5000, daily:'5.5%', days:20, desc:'For serious investors'},
  {id:7, name:'Elite', price:7000, daily:'6%', days:25, desc:'Top-tier returns'}
];

/* ---------------- Render Plans ---------------- */
function renderPlans(){
  const wrap = document.getElementById('plansWrap');
  wrap.innerHTML = '';
  plans.forEach(p=>{
    const card = el(`
      <div class="plan-card card flex items-center justify-between">
        <div>
          <div style="font-weight:800">${p.name} <span class="small muted">• ${p.days} days</span></div>
          <div class="small muted mt-2">${p.desc}</div>
          <div class="mt-2"><strong style="font-size:18px">${money(p.price)}</strong> <span class="small muted">• ${p.daily} / day</span></div>
        </div>
        <div class="flex flex-col items-end gap-2">
          <button class="btn-primary" data-id="${p.id}">Buy</button>
          <button class="btn-ghost small muted" data-id="${p.id}" onclick="viewDetails(event, ${p.id})">Details</button>
        </div>
      </div>
    `);
    wrap.appendChild(card);
  });

  // attach buy handlers
  document.querySelectorAll('.btn-primary').forEach(btn=>{
    btn.onclick = (e)=>{
      const id = Number(btn.dataset.id);
      const p = plans.find(x=>x.id===id);
      openDepositModal(p);
    };
  });
}
function viewDetails(ev, id){
  ev.stopPropagation();
  const p = plans.find(x=>x.id===id);
  alert(`${p.name}\nPrice: ${p.price} PKR\nDaily: ${p.daily}\nDuration: ${p.days} days\n\n${p.desc}`);
}

/* ---------------- Auth Modal ---------------- */
const authRoot = $('authRoot');
document.getElementById('openAuthBtn').addEventListener('click', ()=> showAuthModal());

function showAuthModal(){
  authRoot.innerHTML = '';
  authRoot.appendChild(el(`
    <div id="authModal" style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:60">
      <div style="position:absolute;inset:0;background:rgba(0,0,0,0.6)" onclick="document.getElementById('authModal')?.remove()"></div>
      <div class="card" style="width:420px;z-index:70">
        <div style="display:flex;align-items:center;justify-content:space-between">
          <div>
            <div style="font-weight:800">Account</div>
            <div class="small muted">Login or create new</div>
          </div>
          <button class="btn-ghost" onclick="document.getElementById('authModal')?.remove()">Close</button>
        </div>

        <div id="authTabs" class="mt-3">
          <div id="loginPanel">
            <input id="loginEmail" placeholder="Email (Gmail preferred)">
            <input id="loginPass" type="password" placeholder="Password">
            <div class="mt-3" style="display:flex;gap:8px">
              <button id="doLogin" class="btn-primary" style="flex:1">Login</button>
              <button id="toSignup" class="btn-ghost" style="flex:1">Signup</button>
            </div>
          </div>

          <div id="signupPanel" class="hidden">
            <input id="signupName" placeholder="Username">
            <input id="signupEmail" placeholder="Email">
            <input id="signupPass" type="password" placeholder="Password">
            <div class="mt-3" style="display:flex;gap:8px">
              <button id="doSignup" class="btn-primary" style="flex:1">Create</button>
              <button id="toLogin" class="btn-ghost" style="flex:1">Back</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  `));

  // handlers
  $('toSignup').onclick = ()=>{ $('loginPanel').classList.add('hidden'); $('signupPanel').classList.remove('hidden'); };
  $('toLogin').onclick = ()=>{ $('signupPanel').classList.add('hidden'); $('loginPanel').classList.remove('hidden'); };

  $('doLogin').onclick = ()=> {
    const email = $('loginEmail').value.trim();
    const pass = $('loginPass').value.trim();
    if(!email || !pass) return alert('Fill email & password');
    const u = users.find(x=>x.email === email && x.password === pass);
    if(!u) return alert('Invalid credentials');
    currentUser = u; saveState(); document.getElementById('authModal')?.remove(); refreshUI(); alert('Logged in');
  };

  $('doSignup').onclick = ()=> {
    const name = $('signupName').value.trim();
    const email = $('signupEmail').value.trim();
    const pass = $('signupPass').value.trim();
    if(!name || !email || !pass) return alert('Fill all fields');
    if(users.find(x=>x.email === email)) return alert('Email already used');
    const nu = { name, email, password: pass, balance: 0, deposits: [], withdrawals: [] };
    users.push(nu); currentUser = nu; saveState(); document.getElementById('authModal')?.remove(); refreshUI(); alert('Account created & logged in');
  };
}

/* ---------------- Profile / History ---------------- */
function renderProfile(){
  const area = $('profileArea');
  if(!currentUser){ area.innerHTML = '<div class="small muted">Not signed in</div>'; return; }
  area.innerHTML = `
    <div><strong>${currentUser.name}</strong></div>
    <div class="small muted">${currentUser.email}</div>
    <div class="mt-3 small muted"><strong>Balance:</strong> ${money(currentUser.balance || 0)}</div>
    <div class="mt-3"><button class="btn-primary w-full" onclick="openProfileModal()">Open Profile Page</button></div>
  `;
}
document.getElementById('btnProfile').onclick = ()=> openProfileModal();
function openProfileModal(){
  if(!currentUser){ alert('Please login first'); showAuthModal(); return; }
  modalRoot.innerHTML = '';
  modalRoot.appendChild(el(`
    <div id="profileModal" style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:50">
      <div style="position:absolute;inset:0;background:rgba(0,0,0,0.6)" onclick="document.getElementById('profileModal')?.remove()"></div>
      <div class="card" style="width:820px;max-height:80vh;overflow:auto;z-index:60">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div><div style="font-weight:800">${currentUser.name}</div><div class="small muted">${currentUser.email}</div></div>
          <div><button class="btn-ghost" onclick="logout()">Logout</button></div>
        </div>

        <div style="margin-top:12px; display:grid; grid-template-columns:1fr 1fr; gap:12px">
          <div class="card">
            <div class="small muted">Balance</div>
            <div style="font-weight:800; margin-top:6px">${money(currentUser.balance||0)}</div>
            <div class="mt-3 small muted">Deposits history</div>
            <div id="histDeposits" class="mt-2"></div>
          </div>

          <div class="card">
            <div class="small muted">Withdrawals</div>
            <div id="histWithdrawals" class="mt-2"></div>
          </div>
        </div>

        <div style="margin-top:12px;display:flex;gap:8px">
          <button class="btn-primary" onclick="closeProfile()">Close</button>
          <button class="btn-ghost" onclick="copyReferral()">Copy Referral Link</button>
        </div>
      </div>
    </div>
  `));
  renderHistories();
}
function renderHistories(){
  const hd = $('histDeposits'); hd.innerHTML = '';
  const hw = $('histWithdrawals'); hw.innerHTML = '';
  if(currentUser.deposits && currentUser.deposits.length){
    currentUser.deposits.forEach(d=>{
      const item = el(`<div class="small muted" style="margin-bottom:8px"><div><strong>${d.method.toUpperCase()}</strong> • ${money(d.amount)} • ${new Date(d.date).toLocaleString()}</div><div class="small muted">TX: ${d.txid}</div></div>`);
      hd.appendChild(item);
    });
  } else hd.innerHTML = '<div class="small muted">No deposits yet</div>';

  if(currentUser.withdrawals && currentUser.withdrawals.length){
    currentUser.withdrawals.forEach(w=>{
      const item = el(`<div class="small muted" style="margin-bottom:8px"><div><strong>${w.method.toUpperCase()}</strong> • ${money(w.amount)} • ${new Date(w.date).toLocaleString()}</div><div class="small muted">To: ${w.number}</div></div>`);
      hw.appendChild(item);
    });
  } else hw.innerHTML = '<div class="small muted">No withdrawals yet</div>';
}
function closeProfile(){ document.getElementById('profileModal')?.remove(); }
function logout(){ if(!confirm('Logout?')) return; currentUser = null; saveState(); refreshUI(); document.getElementById('profileModal')?.remove(); }

/* ---------------- Deposit Modal ---------------- */
const modalRoot = $('modalRoot');
document.getElementById('btnDeposit').onclick = ()=> openDepositModal();
function openDepositModal(prefPlan = null){
  if(prefPlan && !prefPlan.price) prefPlan = null;
  if(!currentUser){ alert('Please login/signup first'); showAuthModal(); return; }

  modalRoot.innerHTML = '';
  modalRoot.appendChild(el(`
    <div id="depositModal" style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:55">
      <div style="position:absolute;inset:0;background:rgba(0,0,0,0.6)" onclick="document.getElementById('depositModal')?.remove()"></div>
      <div class="card" style="width:520px;z-index:60">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div><div style="font-weight:800">New Deposit</div><div class="small muted">Send to the chosen method & upload proof</div></div>
          <button class="btn-ghost" onclick="document.getElementById('depositModal')?.remove()">Close</button>
        </div>

        <div class="mt-3">
          <label class="small muted">Select Plan (optional)</label>
          <select id="selPlan" class="mt-2">
            <option value="">-- No plan (custom amount) --</option>
            ${plans.map(p=>`<option value="${p.id}">${p.name} • ${p.days} days • ${money(p.price)}</option>`).join('')}
          </select>

          <label class="small muted mt-3">Method</label>
          <div style="display:flex;gap:8px;margin-top:8px">
            <button id="mJazz" class="btn-ghost" style="flex:1">JazzCash • 03705519562</button>
            <button id="mEasy" class="btn-ghost" style="flex:1">EasyPaisa • 03379827882</button>
          </div>

          <label class="small muted mt-3">Amount</label>
          <input id="depAmount" type="number" placeholder="Enter amount or select plan">

          <label class="small muted mt-2">Transaction ID / Reference</label>
          <input id="depTX" placeholder="Enter TXID / Ref">

          <label class="small muted mt-2">Upload Proof (image)</label>
          <input id="depFile" type="file" accept="image/*">
          <img id="depPreview" class="file-preview hidden">

          <div style="display:flex;gap:8px;margin-top:12px">
            <button id="submitDep" class="btn-primary" style="flex:1">Submit Deposit</button>
            <button id="clearDep" class="btn-ghost" style="flex:1">Clear</button>
          </div>
        </div>
      </div>
    </div>
  `));

  // initial selection
  let method = 'jazz';
  $('mJazz').classList.add('btn-primary'); $('mEasy').classList.remove('btn-primary');

  $('mJazz').onclick = ()=>{ method='jazz'; $('mJazz').classList.add('btn-primary'); $('mEasy').classList.remove('btn-primary'); };
  $('mEasy').onclick = ()=>{ method='easy'; $('mEasy').classList.add('btn-primary'); $('mJazz').classList.remove('btn-primary'); };

  // plan change
  $('selPlan').onchange = (e)=>{
    const id = Number(e.target.value);
    if(!id){ $('depAmount').value = ''; return; }
    const p = plans.find(x=>x.id===id); if(p) $('depAmount').value = p.price;
  };

  // preview file
  $('depFile').onchange = (ev)=>{
    const f = ev.target.files[0]; if(!f){ $('depPreview').classList.add('hidden'); return; }
    $('depPreview').src = URL.createObjectURL(f); $('depPreview').classList.remove('hidden');
  };

  $('clearDep').onclick = ()=>{ $('depAmount').value=''; $('depTX').value=''; $('depFile').value=''; $('depPreview').classList.add('hidden'); $('selPlan').value=''; };

  $('submitDep').onclick = ()=>{
    const amt = Number($('depAmount').value);
    const tx = $('depTX').value.trim();
    if(!amt || !tx) return alert('Fill amount and transaction ID');
    const file = $('depFile').files[0];
    const dep = { amount:amt, txid:tx, method, date: new Date().toISOString(), proof:null, planId: Number($('selPlan').value)||null, status:'Pending' };
    if(file){
      const r = new FileReader();
      r.onload = ev => { dep.proof = ev.target.result; finalizeDeposit(dep); document.getElementById('depositModal')?.remove(); };
      r.readAsDataURL(file);
    } else { finalizeDeposit(dep); document.getElementById('depositModal')?.remove(); }
  };
}

function finalizeDeposit(dep){
  if(!currentUser) return alert('Please login');
  currentUser.deposits = currentUser.deposits || [];
  currentUser.withdrawals = currentUser.withdrawals || [];
  currentUser.deposits.unshift(dep);
  currentUser.balance = (Number(currentUser.balance) || 0) + Number(dep.amount);
  saveState(); refreshUI();
  alert('Deposit recorded. Status: Pending (demo).');
}

/* ---------------- Withdraw Modal ---------------- */
document.getElementById('btnWithdraw').onclick = ()=> {
  if(!currentUser){ alert('Please login first'); showAuthModal(); return; }
  modalRoot.innerHTML = '';
  modalRoot.appendChild(el(`
    <div id="withdrawModal" style="position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:55">
      <div style="position:absolute;inset:0;background:rgba(0,0,0,0.6)" onclick="document.getElementById('withdrawModal')?.remove()"></div>
      <div class="card" style="width:480px;z-index:60">
        <div style="display:flex;justify-content:space-between;align-i
