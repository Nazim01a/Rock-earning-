<ROCK>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Rock Earning — Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{
    --bg:#0b1020; --card:#0f1724; --muted:#94a3b8; --accent1:#6ee7b7; --accent2:#60a5fa;
  }
  [data-theme="light"]{
    --bg: #f8fafc; --card:#ffffff; --muted:#475569; --accent1:#10b981; --accent2:#3b82f6;
    color-scheme: light;
  }
  body{ background: linear-gradient(135deg, rgba(9,10,25,1) 0%, rgba(12,14,30,1) 100%); min-height:100vh;
    font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;
    background-color:var(--bg); color: #e6eef8;
  }
  .card{ background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
         border-radius:14px; box-shadow: 0 8px 30px rgba(2,6,23,0.6); padding:1rem; }
  .glass{ backdrop-filter: blur(6px); -webkit-backdrop-filter: blur(6px); background: rgba(255,255,255,0.03); }
  .btn-grad{ background: linear-gradient(90deg,var(--accent2),var(--accent1)); color:#05263a; }
  .pulse { animation: pulse 2s infinite; }
  @keyframes slideUp { from { transform: translateY(30px); opacity:0 } to { transform: translateY(0); opacity:1 } }
  .slide-up { animation: slideUp .36s ease-out both; }
  .modal-enter { animation: modalIn .28s cubic-bezier(.2,.9,.2,1) both; }
  @keyframes modalIn { from { transform: translateY(20px) scale(.98); opacity:0 } to { transform: translateY(0) scale(1); opacity:1 } }
  .plan-hover:hover{ transform: translateY(-6px) scale(1.01); transition: all .25s ease; box-shadow: 0 12px 30px rgba(2,6,23,0.6); }
  .logo { width:44px;height:44px;border-radius:10px;background:linear-gradient(45deg,var(--accent2),var(--accent1)); display:flex;align-items:center;justify-content:center;font-weight:700;color:#05263a; }
  input,select,button{ outline:none; }
  /* responsive tweak for mobile modals */
  @media (max-width:640px){ .modal-box{ width:95% } }
</style>
</head>
<body data-theme="dark" class="px-4 py-6">

<!-- TOP NAV -->
<nav class="max-w-6xl mx-auto flex items-center justify-between gap-4 mb-6">
  <div class="flex items-center gap-3">
    <div class="logo">RE</div>
    <div>
      <div class="text-xl font-semibold">Rock Earning</div>
      <div class="text-sm" style="color:var(--muted)">Premium Demo</div>
    </div>
  </div>

  <div class="flex items-center gap-3">
    <button id="themeToggle" class="px-3 py-2 rounded glass text-sm">Toggle Light</button>
    <button id="openAuth" class="px-3 py-2 rounded glass text-sm">Login / Signup</button>
  </div>
</nav>

<!-- MAIN CONTAINER -->
<div id="app" class="max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-3 gap-6">

  <!-- LEFT: BALANCE + ACTIONS -->
  <section id="leftPanel" class="card glass slide-up lg:col-span-1">
    <div class="flex items-center justify-between">
      <div>
        <div class="text-sm" style="color:var(--muted)">Available Balance</div>
        <div id="balance" class="text-3xl font-bold mt-1">₨ 0</div>
      </div>
      <div class="text-right">
        <div id="welcomeUser" class="text-sm opacity-90">Welcome</div>
        <div class="text-xs" style="color:var(--muted)">Client-side Demo</div>
      </div>
    </div>

    <div class="mt-5 space-y-3">
      <button id="openDeposit" class="w-full py-2 rounded btn-grad font-semibold">Deposit (Easypaisa / JazzCash)</button>
      <button id="openWithdraw" class="w-full py-2 rounded bg-yellow-400 text-black font-semibold">Withdraw</button>
      <button id="copyReferral" class="w-full py-2 rounded glass text-sm">Copy Referral Link</button>
    </div>

    <div class="mt-5 text-sm" style="color:var(--muted)">
      <div class="font-medium">Quick Info</div>
      <ul class="mt-2 space-y-1">
        <li>• Deposit proof upload supported</li>
        <li>• Withdraw requests simulated (demo)</li>
        <li>• JazzCash: <span class="font-mono">03705519562</span></li>
        <li>• Easypaisa: <span class="font-mono">03379827882</span></li>
      </ul>
    </div>
  </section>

  <!-- MIDDLE: PLANS -->
  <section id="plansPanel" class="card glass slide-up lg:col-span-1">
    <div class="flex items-center justify-between mb-3">
      <h3 class="text-lg font-semibold">Investment Plans</h3>
      <div class="text-sm" style="color:var(--muted)">Choose & Buy</div>
    </div>

    <div id="plans" class="grid grid-cols-1 gap-3">
      <!-- JS will render plans -->
    </div>

    <div class="mt-4 text-center text-sm" style="color:var(--muted)">
      <span class="italic">New plans rolling out — keep checking!</span>
    </div>
  </section>

  <!-- RIGHT: Deposit History -->
  <section class="card glass slide-up lg:col-span-1">
    <div class="flex items-center justify-between mb-3">
      <h3 class="text-lg font-semibold">Deposits</h3>
      <div class="text-sm" style="color:var(--muted)"><button id="refreshList" class="underline">Refresh</button></div>
    </div>
    <div id="depositsList" class="space-y-3 max-h-[52vh] overflow-auto"></div>
  </section>

</div>

<!-- FOOTER -->
<footer class="max-w-6xl mx-auto mt-6 text-center text-sm" style="color:var(--muted)">
  Rock Earning — Demo • Not real money system
</footer>

<!-- AUTH MODAL (hidden by default) -->
<div id="authModalRoot"></div>

<!-- DEPOSIT MODAL ROOT -->
<div id="modalRoot"></div>

<script>
/* ---------- App State (localStorage) ---------- */
let users = JSON.parse(localStorage.getItem('rockUsers') || '[]');
let currentUser = JSON.parse(localStorage.getItem('rockCurrentUser') || 'null');

/* ---------- Utilities ---------- */
function saveAll(){
  localStorage.setItem('rockUsers', JSON.stringify(users));
  localStorage.setItem('rockCurrentUser', JSON.stringify(currentUser));
}
function money(n){ return '₨ ' + Number(n).toLocaleString(); }
function el(html){ const d=document.createElement('div'); d.innerHTML=html.trim(); return d.firstChild; }

/* ---------- Theme Toggle ---------- */
const themeToggle = document.getElementById('themeToggle');
themeToggle.onclick = () => {
  const root = document.body;
  if(root.getAttribute('data-theme') === 'light'){ root.setAttribute('data-theme','dark'); themeToggle.textContent='Toggle Light'; }
  else { root.setAttribute('data-theme','light'); themeToggle.textContent='Toggle Dark'; }
};

/* ---------- Auth (login/signup) ---------- */
const authRoot = document.getElementById('authModalRoot');
document.getElementById('openAuth').addEventListener('click', showAuthModal);

function showAuthModal(){
  authRoot.innerHTML = '';
  authRoot.appendChild(el(`
    <div id="authModal" class="fixed inset-0 z-50 flex items-center justify-center">
      <div class="absolute inset-0 bg-black/60" onclick="document.getElementById('authModal').remove()"></div>
      <div class="modal-box bg-white text-slate-900 rounded-xl p-5 w-full max-w-md z-10 slide-up">
        <div class="flex items-center justify-between mb-3">
          <div class="flex items-center gap-3">
            <div class="logo" style="background:linear-gradient(45deg,var(--accent2),var(--accent1));color:#05263a">RE</div>
            <div>
              <div class="font-semibold">Account</div>
              <div class="text-xs" style="color:var(--muted)">Login or Create</div>
            </div>
          </div>
          <button onclick="document.getElementById('authModal').remove()" class="text-sm text-gray-500">Close</button>
        </div>

        <div id="authTabs" class="space-y-4">
          <div id="loginPanel">
            <input id="loginEmail" placeholder="Gmail" class="w-full mb-2 p-2 border rounded" />
            <input id="loginPassword" type="password" placeholder="Password" class="w-full mb-3 p-2 border rounded" />
            <div class="flex gap-2">
              <button id="doLogin" class="flex-1 py-2 rounded btn-grad">Login</button>
              <button id="toSignup" class="flex-1 py-2 rounded bg-gray-200">Signup</button>
            </div>
          </div>

          <div id="signupPanel" class="hidden">
            <input id="signupName" placeholder="Username" class="w-full mb-2 p-2 border rounded" />
            <input id="signupEmail" placeholder="Gmail" class="w-full mb-2 p-2 border rounded" />
            <input id="signupPassword" type="password" placeholder="Password" class="w-full mb-3 p-2 border rounded" />
            <div class="flex gap-2">
              <button id="createAcc" class="flex-1 py-2 rounded bg-green-600 text-white">Create</button>
              <button id="toLogin" class="flex-1 py-2 rounded bg-gray-200">Back</button>
            </div>
          </div>

        </div>
      </div>
    </div>
  `));

  // Handlers
  document.getElementById('toSignup').onclick = ()=>{ document.getElementById('loginPanel').classList.add('hidden'); document.getElementById('signupPanel').classList.remove('hidden'); };
  document.getElementById('toLogin').onclick = ()=>{ document.getElementById('signupPanel').classList.add('hidden'); document.getElementById('loginPanel').classList.remove('hidden'); };

  document.getElementById('doLogin').onclick = ()=>{
    const email = document.getElementById('loginEmail').value.trim();
    const pass = document.getElementById('loginPassword').value.trim();
    const u = users.find(x=>x.email===email && x.password===pass);
    if(!u){ alert('Invalid details'); return; }
    currentUser = u;
    saveAll(); refreshUI();
    document.getElementById('authModal').remove();
  };

  document.getElementById('createAcc').onclick = ()=>{
    const name = document.getElementById('signupName').value.trim();
    const email = document.getElementById('signupEmail').value.trim();
    const pass = document.getElementById('signupPassword').value.trim();
    if(!name||!email||!pass){ alert('Fill all'); return; }
    if(users.find(x=>x.email===email)){ alert('Email already used'); return; }
    const newU = { name, email, password: pass, balance: 0, deposits: [], withdrawals: [] };
    users.push(newU); currentUser = newU; saveAll(); refreshUI();
    alert('Signup successful');
    document.getElementById('authModal').remove();
  };
}

/* ---------- Plans (7 plans) ---------- */
const plansData = [
  { id:1, name:'Starter', amount:180, daily:'3%', days:10, desc:'Low entry, steady returns' },
  { id:2, name:'Bronze', amount:500, daily:'3.5%', days:12, desc:'Beginner-friendly' },
  { id:3, name:'Silver', amount:1000, daily:'4%', days:14, desc:'Balanced growth' },
  { id:4, name:'Gold', amount:1500, daily:'4.5%', days:18, desc:'Popular choice' },
  { id:5, name:'Platinum', amount:2500, daily:'5%', days:20, desc:'High return tier' },
  { id:6, name:'Diamond', amount:5000, daily:'5.5%', days:22, desc:'Pro investors' },
  { id:7, name:'Elite', amount:7000, daily:'6%', days:25, desc:'Top tier' }
];

function renderPlans(){
  const wrap = document.getElementById('plans');
  wrap.innerHTML = '';
  plansData.forEach(p=>{
    const active = true; // all active in demo; you can set some to coming soon
    const card = el(`
      <div class="plan-hover p-3 border rounded flex justify-between items-center glass">
        <div>
          <div class="font-semibold">${p.name} <span class="ml-2 text-xs" style="color:var(--muted)">${p.days} days</span></div>
          <div class="text-sm" style="color:var(--muted)">${p.desc}</div>
          <div class="mt-1 text-sm"><span class="font-bold">${money(p.amount)}</span> • ${p.daily} daily</div>
        </div>
        <div class="flex flex-col items-end gap-2">
          <button class="buyBtn px-3 py-1 rounded btn-grad" data-id="${p.id}">Buy</button>
          <button class="text-xs" style="color:var(--muted)">${active ? 'Active' : 'Coming Soon'}</button>
        </div>
      </div>
    `);
    wrap.appendChild(card);
  });

  // attach buy handlers
  document.querySelectorAll('.buyBtn').forEach(b=>{
    b.onclick = ()=> {
      const pid = Number(b.dataset.id);
      const plan = plansData.find(x=>x.id===pid);
      openDepositModal(plan.amount, plan);
    };
  });
}

/* ---------- Deposit Modal ---------- */
const modalRoot = document.getElementById('modalRoot');

document.getElementById('openDeposit').addEventListener('click', ()=> openDepositModal());

function openDepositModal(prefAmount = '', plan = null){
  const planInfo = plan ? `<div class="mb-2 text-sm">Buying: <b>${plan.name}</b></div>` : '';
  modalRoot.innerHTML = '';
  modalRoot.appendChild(el(`
    <div id="depositModal" class="fixed inset-0 z-50 flex items-center justify-center">
      <div class="absolute inset-0 bg-black/60" onclick="document.getElementById('depositModal').remove()"></div>
      <div class="modal-box w-full max-w-lg bg-white text-slate-900 rounded-xl p-5 z-10 modal-enter">
        <div class="flex items-center justify-between mb-3">
          <div><div class="font-semibold">New Deposit</div><div class="text-xs" style="color:var(--muted)">Upload proof after sending</div></div>
          <button onclick="document.getElementById('depositModal').remove()" class="text-sm text-gray-500">Close</button>
        </div>

        ${planInfo}

        <div class="grid grid-cols-1 gap-2">
          <label class="text-sm">Method</label>
          <div class="flex gap-2">
            <button id="dmE" class="flex-1 p-2 border rounded">Easypaisa • 03379827882</button>
            <button id="dmJ" class="flex-1 p-2 border rounded">JazzCash • 03705519562</button>
          </div>

          <input id="depAmount" type="number" placeholder="Amount (₨)" class="p-2 border rounded" value="${prefAmount || ''}">
          <input id="depTX" placeholder="Transaction ID / Ref" class="p-2 border rounded" />
          <input id="depFile" type="file" accept="image/*" class="p-1" />
          <div id="depPreview" class="hidden"><img id="depPreviewImg" class="max-w-full rounded border mt-2" /></div>

          <div class="flex gap-2 mt-3">
            <button id="depSubmit" class="flex-1 py-2 rounded btn-grad">Submit Deposit</button>
            <button id="depClear" class="flex-1 py-2 rounded bg-gray-200">Clear</button>
          </div>

        </div>
      </div>
    </div>
  `));

  // method toggle
  let selected = 'easypaisa';
  document.getElementById('dmE').onclick = ()=>{ selected='easypaisa'; document.getElementById('dmE').classList.add('border-2'); document.getElementById('dmJ').classList.remove('border-2'); };
  document.getElementById('dmJ').onclick = ()=>{ selected='jazzcash'; document.getElementById('dmJ').classList.add('border-2'); document.getElementById('dmE').classList.remove('border-2'); };

  // preview
  const depFile = document.getElementById('depFile');
  depFile.onchange = (e)=>{
    const f = e.target.files[0];
    if(!f) { document.getElementById('depPreview').classList.add('hidden'); return; }
    document.getElementById('depPreviewImg').src = URL.createObjectURL(f);
    document.getElementById('depPreview').classList.remove('hidden');
  };

  document.getElementById('depClear').onclick = ()=>{
    document.getElementById('depAmount').value=''; document.getElementById('depTX').value=''; depFile.value=''; document.getElementById('depPreview').classList.add('hidden');
  };

  document.getElementById('depSubmit').onclick = ()=>{
    if(!currentUser){ alert('Please login or signup first'); return; }
    const amt = Number(document.getElementById('depAmount').value);
    const tx = document.getElementById('depTX').value.trim();
    if(!amt || !tx){ alert('Fill amount and TXID'); return; }
    const file = depFile.files[0];
    const deposit = { amount: amt, txid: tx, method: selected, status: 'Pending', date: new Date().toISOString(), proof: null, plan: plan ? plan.name : null };
    if(file){
      const r = new FileReader();
      r.onload = (ev)=>{ deposit.proof = ev.target.result; finalizeDeposit(deposit); document.getElementById('depositModal').remove(); };
      r.readAsDataURL(file);
    } else { finalizeDeposit(deposit); document.getElementById('depositModal').remove(); }
  };
}

function finalizeDeposit(dep){
  currentUser.deposits.unshift(dep);
  currentUser.balance = Number(currentUser.balance) + Number(dep.amount);
  saveAll(); refreshUI();
  alert('Deposit submitted (demo).');
}

/* ---------- Withdraw Modal (professional) ---------- */
document.getElementById('openWithdraw').addEventListener('click', ()=>{
  if(!currentUser){ alert('Login first'); return; }
  modalRoot.innerHTML = '';
  modalRoot.appendChild(el(`
    <div id="withdrawModal" class="fixed inset-0 z-50 flex items-end sm:items-center justify-center">
      <div class="absolute inset-0 bg-black/60" onclick="document.getElementById('withdrawModal').remove()"></div>
      <div class="modal-box w-full max-w-md bg-white text-slate-900 rounded-t-xl sm:rounded-xl p-5 z-10 modal-enter">
        <div class="flex items-center justify-between mb-3">
          <div>
            <div class="font-semibold">Withdraw Request</div>
            <div class="text-xs" style="color:var(--muted)">Fast & secure (demo)</div>
          </div>
          <button onclick="document.getElementById('withdrawModal').remove()" class="text-sm text-gray-500">Close</button>
        </div>

        <div class="space-y-2">
          <label class="text-sm">Method</label>
          <select id="wMethod" class="w-full p-2 border rounded">
            <option value="easypaisa">Easypaisa</option>
            <option value="jazzcash">JazzCash</option>
          </select>

          <label class="text-sm">Account Number</label>
          <input id="wNumber" class="w-full p-2 border rounded" placeholder="03xxxxxxxxx" />

          <label class="text-sm">Amount (₨)</label>
          <input id="wAmount" type="number" class="w-full p-2 border rounded" placeholder="Enter amount" />

          <label class="text-sm">Optional Note</label>
          <input id="wNote" class="w-full p-2 border rounded" placeholder="e.g. Withdraw for expenses" />

          <div class="mt-3 flex justify-between gap-2">
            <button id="cancelW" class="flex-1 py-2 rounded bg-gray-200">Cancel</button>
            <button id="submitW" class="flex-1 py-2 rounded btn-grad">Submit</button>
          </div>
        </div>

      </div>
    </div>
  `));

  document.getElementById('cancelW').onclick = ()=> document.getElementById('withdrawModal').remove();

  document.getElementById('submitW').onclick = ()=>{
    const amt = Number(document.getElementById('wAmount').value);
    const num = document.getElementById('wNumber').value.trim();
    const method = document.getElementById('wMethod').value;
    const note = document.getElementById('wNote').value.trim();
    if(!amt || !num){ alert('Fill all required fields'); return; }
    if(amt > Number(currentUser.balance)){ alert('Insufficient balance'); return; }
    // create withdrawal record
    const w = { amount: amt, number: num, method, note, status: 'Requested', date: new Date().toISOString() };
    currentUser.withdrawals.unshift(w);
    currentUser.balance = Number(currentUser.balance) - Number(amt);
    saveAll(); refreshUI();
    alert('Withdraw request submitted (demo).');
    document.getElementById('withdrawModal').remove();
  };
});

/* ---------- Deposits List & UI Refresh ---------- */
function renderDeposits(){
  const list = document.getElementById('depositsList');
  list.innerHTML = '';
  if(!currentUser || !currentUser.deposits || currentUser.deposits.length === 0){
    list.innerHTML = `<div class="text-sm" style="color:var(--muted)">No deposits yet</div>`; return;
  }
  currentUser.deposits.forEach(d=>{
    const item = el(`
      <div class="p-3 border rounded glass">
        <div class="flex items-start justify-between">
          <div>
            <div class="font-semibold">${d.method.toUpperCase()} • ${money(d.amount)}</div>
            <div class="text-xs" style="color:var(--muted)">${new Date(d.date).toLocaleString()}</div>
            ${d.plan ? `<div class="text-xs mt-1">Plan: <b>${d.plan}</b></div>` :
