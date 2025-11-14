<Hi😘><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Pro — Final Version</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{
  font-family:'Segoe UI',sans-serif;
  background:#0b0f1a;
  color:white;
}
.neon-card{
  transition:0.3s;
  border:1px solid rgba(255,255,255,0.1);
  background:rgba(255,255,255,0.05);
  backdrop-filter:blur(15px);
}
.neon-card:hover{
  transform:scale(1.05);
  box-shadow:0 0 25px rgba(0,150,255,0.7);
}
#sidePanel{
  position:fixed;
  top:0;
  right:-400px;
  width:350px;
  height:100vh;
  background:#0f172a;
  padding:20px;
  transition:0.4s;
  overflow-y:auto;
  z-index:999;
}
</style>
</head>
<body class="p-4"><!-- HEADER --><header class="text-center py-6">
  <h1 class="text-4xl font-bold">Rock Earn Pro Dashboard</h1>
  <p class="opacity-70">Since 2018 • Crypto FinTech • Partnered with Binance</p>
</header><!-- SIDE MENU --><div class="flex gap-4 flex-wrap justify-center mt-4">
  <button onclick="openPanel('profile')" class="bg-blue-600 px-4 py-2 rounded-lg">Profile</button>
  <button onclick="openPanel('deposit')" class="bg-green-600 px-4 py-2 rounded-lg">Deposit</button>
  <button onclick="openPanel('withdraw')" class="bg-yellow-600 px-4 py-2 rounded-lg">Withdraw</button>
  <button onclick="openPanel('activity')" class="bg-purple-600 px-4 py-2 rounded-lg">Activity</button>
  <button onclick="openPanel('company')" class="bg-gray-600 px-4 py-2 rounded-lg">Company</button>
  <button onclick="openPanel('settings')" class="bg-red-600 px-4 py-2 rounded-lg">Settings</button>
</div><!-- PLANS SECTION --><section class="mt-10 text-center">
  <h2 class="text-3xl font-bold mb-6">Our Investment Plans</h2>  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"><!-- 6 ACTIVE PLANS -->
<div class="neon-card p-6 rounded-2xl">
  <h3 class="text-xl font-semibold mb-1">Starter Plan</h3>
  <p>Amount: 180 PKR</p>
  <p>Daily Profit: 20 PKR</p>
  <button onclick="buyPlan(180)" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg">Buy Now</button>
</div>

<div class="neon-card p-6 rounded-2xl">
  <h3 class="text-xl font-semibold mb-1">Basic Plan</h3>
  <p>Amount: 500 PKR</p>
  <p>Daily Profit: 60 PKR</p>
  <button onclick="buyPlan(500)" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg">Buy Now</button>
</div>

<div class="neon-card p-6 rounded-2xl">
  <h3 class="text-xl font-semibold mb-1">Standard Plan</h3>
  <p>Amount: 1000 PKR</p>
  <p>Daily Profit: 130 PKR</p>
  <button onclick="buyPlan(1000)" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg">Buy Now</button>
</div>

<div class="neon-card p-6 rounded-2xl">
  <h3 class="text-xl font-semibold mb-1">Premium Plan</h3>
  <p>Amount: 2500 PKR</p>
  <p>Daily Profit: 320 PKR</p>
  <button onclick="buyPlan(2500)" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg">Buy Now</button>
</div>

<div class="neon-card p-6 rounded-2xl">
  <h3 class="text-xl font-semibold mb-1">Gold Plan</h3>
  <p>Amount: 5000 PKR</p>
  <p>Daily Profit: 650 PKR</p>
  <button onclick="buyPlan(5000)" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg">Buy Now</button>
</div>

<div class="neon-card p-6 rounded-2xl">
  <h3 class="text-xl font-semibold mb-1">Elite Plan</h3>
  <p>Amount: 7000 PKR</p>
  <p>Daily Profit: 900 PKR</p>
  <button onclick="buyPlan(7000)" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg">Buy Now</button>
</div>

<!-- COMING SOON PLANS -->
<div class="neon-card p-6 rounded-2xl opacity-50">
  <h3 class="text-xl font-semibold mb-1">Mega Booster (Coming Soon)</h3>
  <p>Amount: 10,000 PKR</p>
  <p>Daily Profit: 1,500 PKR</p>
</div>

<div class="neon-card p-6 rounded-2xl opacity-50">
  <h3 class="text-xl font-semibold mb-1">Ultra Pro (Coming Soon)</h3>
  <p>Amount: 15,000 PKR</p>
  <p>Daily Profit: 2,300 PKR</p>
</div>

<div class="neon-card p-6 rounded-2xl opacity-50">
  <h3 class="text-xl font-semibold mb-1">Crypto Miner (Coming Soon)</h3>
  <p>Amount: 20,000 PKR</p>
  <p>Daily Profit: 3,500 PKR</p>
</div>

  </div>
</section><!-- SIDE PANEL --><div id="sidePanel"></div><script>
function openPanel(type){
  let panel = document.getElementById('sidePanel');
  panel.style.right = '0px';

  if(type === 'profile'){
    panel.innerHTML = `
      <h2 class='text-2xl font-bold mb-4'>Your Profile</h2>
      <p>Name: User</p>
      <p>ID: RE-928373</p>
      <button class='bg-blue-600 px-3 py-1 rounded mt-2' onclick="copyText('https://rockearnpro.com/user/928373')">Copy Profile Link</button>
    `;
  }

  if(type === 'deposit'){
    panel.innerHTML = `
      <h2 class='text-2xl font-bold mb-4'>Deposit</h2>
      <p class='mb-2'>Amount Selected: <b id='depositAmount'>0</b> PKR</p>
      <p>JazzCash: 03705519562</p>
      <button class='bg-blue-600 px-3 py-1 rounded' onclick="copyText('03705519562')">Copy</button>
      <p class='mt-3'>EasyPaisa: 03379827882</p>
      <button class='bg-blue-600 px-3 py-1 rounded' onclick="copyText('03379827882')">Copy</button>
      <input type='file' class='mt-3'>
      <input type='text' placeholder='Transaction ID' class='mt-3 p-2 w-full rounded bg-white/20'>
      <button class='bg-green-600 px-4 py-2 rounded mt-4 w-full'>Submit</button>
    `;
  }

  if(type === 'withdraw'){
    panel.innerHTML = `
      <h2 class='text-2xl font-bold mb-4'>Withdraw</h2>
      <input type='number' placeholder='Amount' class='w-full p-2 rounded bg-white/20 mb-3'>
      <select class='w-full p-2 rounded bg-white/20 mb-3'>
        <option>JazzCash</option>
        <option>EasyPaisa</option>
        <option>Bank</option>
      </select>
      <input type='text' placeholder='Account Number' class='w-full p-2 rounded bg-white/20 mb-3'>
      <button class='bg-yellow-600 px-4 py-2 rounded w-full'>Withdraw</button>
    `;
  }

  if(type === 'activity'){
    panel.innerHTML = `
      <h2 class='text-2xl font-bold mb-4'>Activity History</h2>
      <p>• Deposit: 500 PKR</p>
      <p>• Withdraw: 300 PKR</p>
      <p>• Plan Bought: Basic Plan</p>
    `;
  }

  if(type === 'company'){
    panel.innerHTML = `
      <h2 class='text-2xl font-bold mb-4'>Company Details</h2>
      <p>Since: <b>2018</b></p>
      <p>Industry: <b>Crypto & FinTech</b></p>
      <p>Partner: <b>Binance</b></p>
      <p>Address: <b>1234 Crypto Avenue, San Francisco, California, USA</b></p>
      <p>Email: support@rockearnpro.com</p>
    `;
  }

  if(type === 'settings'){
    panel.innerHTML = `
      <h2 class='text-2xl font-bold mb-4'>Settings</h2>
      <input type='text' placeholder='Change Name' class='w-full p-2 rounded bg-white/20 mb-3'>
      <input type='password' placeholder='Change Password' class='w-full p-2 rounded bg-white/20 mb-3'>
      <button class='bg-red-600 px-4 py-2 rounded w-full'>Save</button>
    `;
  }
}

function buyPlan(amount){
  openPanel('deposit');
  document.getElementById('depositAmount').innerText = amount;
}

function copyText(text){
  navigator.clipboard.writeText(text);
  alert("Copied: " + text);
}
</script></body>
</html>
