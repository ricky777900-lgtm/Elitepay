<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Elite Pay | Premium</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        body { background: #000; color: #fff; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .gold-text { color: #FFD700; }
        .gold-bg { background: linear-gradient(135deg, #FFD700 0%, #B8860B 100%); }
        .glass { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.1); backdrop-filter: blur(10px); }
        .tab-content { display: none; }
        .active { display: block; animation: slideUp 0.4s ease-out; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>
</head>
<body>

    <div id="auth-page" class="min-h-screen flex flex-col justify-center p-6 bg-[radial-gradient(circle_at_top,_var(--tw-gradient-stops))] from-zinc-900 via-black to-black">
        <div class="text-center mb-10">
            <h1 class="text-4xl font-black italic gold-text tracking-tighter">ELITE PAY</h1>
            <p class="text-zinc-500 text-xs font-bold uppercase tracking-widest mt-2">Member Authentication</p>
        </div>

        <div class="glass p-8 rounded-[40px] space-y-6">
            <div class="flex gap-4 mb-4">
                <button onclick="toggleAuth('login')" id="login-btn" class="flex-1 pb-2 border-b-2 border-yellow-500 font-bold">Login</button>
                <button onclick="toggleAuth('reg')" id="reg-btn" class="flex-1 pb-2 border-b-2 border-transparent text-zinc-500 font-bold">Register</button>
            </div>
            
            <input type="number" id="auth-phone" placeholder="Phone Number" class="w-full bg-white/5 border border-white/10 p-4 rounded-2xl outline-none focus:border-yellow-500 transition-all">
            <input type="password" id="auth-pass" placeholder="Password" class="w-full bg-white/5 border border-white/10 p-4 rounded-2xl outline-none focus:border-yellow-500 transition-all">
            
            <button onclick="finishAuth()" class="w-full gold-bg text-black font-black py-4 rounded-2xl shadow-xl shadow-yellow-500/20 active:scale-95 transition-all uppercase tracking-widest">Enter Dashboard</button>
        </div>
        <p class="text-center text-[10px] text-zinc-600 mt-10 uppercase font-bold tracking-widest">© 2026 Elite Pay International</p>
    </div>

    <div id="main-app" class="hidden">
        <section id="home" class="tab-content active p-4 pb-24">
            <div class="flex justify-between items-center py-4 mb-6">
                <h2 class="text-2xl font-black italic gold-text">ELITE PAY</h2>
                <div class="bg-zinc-900 px-4 py-1 rounded-full border border-white/5 text-[10px] font-bold">VIP MEMBER</div>
            </div>

            <div class="gold-bg p-8 rounded-[40px] text-black mb-8 shadow-2xl shadow-yellow-500/10">
                <p class="uppercase text-[10px] font-black opacity-60 mb-1 tracking-widest">Available Balance</p>
                <div class="flex items-baseline gap-1">
                    <span class="text-2xl font-bold">₹</span>
                    <span class="text-5xl font-black tracking-tighter">250.00</span>
                </div>
                <div class="mt-6 flex gap-3">
                    <button onclick="showTab('orders')" class="flex-1 bg-black text-white text-[10px] font-bold py-3 rounded-full uppercase tracking-wider">Start Orders</button>
                    <button onclick="alert('Withdrawals open at ₹300')" class="flex-1 bg-black/10 text-black text-[10px] font-bold py-3 rounded-full border border-black/10 uppercase tracking-wider">Withdraw</button>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <a href="https://t.me/Elitepayservice" class="glass p-6 rounded-3xl text-center">
                    <span class="text-2xl">🎧</span>
                    <p class="text-[10px] font-bold uppercase mt-2 text-zinc-400">Support</p>
                </a>
                <a href="https://t.me/+f7Yx9ZeczwNhZGE1" class="glass p-6 rounded-3xl text-center">
                    <span class="text-2xl">📢</span>
                    <p class="text-[10px] font-bold uppercase mt-2 text-zinc-400">Community</p>
                </a>
            </div>
        </section>

        <section id="orders" class="tab-content p-4 pb-24">
            <h2 class="text-2xl font-black mb-6 gold-text italic">MARKETPLACE</h2>
            <div id="orders-grid" class="grid grid-cols-2 gap-4"></div>
        </section>

        <nav class="fixed bottom-6 left-6 right-6 glass rounded-full flex justify-around p-3 z-50">
            <button onclick="showTab('home')" class="p-3 text-yellow-500">🏠</button>
            <button onclick="showTab('orders')" class="p-3 text-zinc-500">🛒</button>
            <button onclick="showTab('profile')" class="p-3 text-zinc-500">👤</button>
        </nav>
    </div>

    <div id="pay-modal" class="fixed inset-0 bg-black/95 hidden z-[100] p-6 overflow-y-auto">
        <button onclick="closePay()" class="bg-zinc-900 text-zinc-400 px-6 py-2 rounded-full text-[10px] font-bold mb-8">✕ BACK</button>
        <div class="bg-white p-6 rounded-[50px] mb-8 mx-auto w-fit shadow-2xl">
            <img src="https://i.postimg.cc/CKcwgZ4P/IMG-20260317-110656-677.jpg" class="w-52 h-52 rounded-2xl">
        </div>
        <input type="number" id="utr" placeholder="Enter 12-digit UTR" class="w-full bg-white/5 border border-white/10 p-5 rounded-2xl text-white outline-none mb-6">
        <button onclick="submitOrder()" class="w-full gold-bg text-black font-black py-5 rounded-2xl">VERIFY PAYMENT</button>
    </div>

    <script>
        function toggleAuth(type) {
            document.getElementById('login-btn').classList.toggle('border-yellow-500', type === 'login');
            document.getElementById('reg-btn').classList.toggle('border-yellow-500', type === 'reg');
        }

        function finishAuth() {
            const phone = document.getElementById('auth-phone').value;
            if(phone.length < 10) return alert("Enter valid phone number");
            document.getElementById('auth-page').classList.add('hidden');
            document.getElementById('main-app').classList.remove('hidden');
        }

        function showTab(t) {
            document.querySelectorAll('.tab-content').forEach(s => s.classList.remove('active'));
            document.getElementById(t).classList.add('active');
        }

        function openPay() { document.getElementById('pay-modal').classList.remove('hidden'); }
        function closePay() { document.getElementById('pay-modal').classList.add('hidden'); }
        function submitOrder() {
            if(document.getElementById('utr').value.length < 12) return alert("Invalid UTR");
            alert("Payment Logged for Approval!"); closePay();
        }

        const grid = document.getElementById('orders-grid');
        for (let i = 0; i < 30; i++) {
            let v = 300 + (i * 162); if(v > 5000) v = 5000;
            grid.innerHTML += `
                <div class="glass p-6 rounded-[35px] flex flex-col items-center">
                    <p class="text-2xl font-black italic">₹${v}</p>
                    <p class="text-[9px] text-green-400 font-bold mb-5">+ ₹${(v*0.045).toFixed(2)} PROFIT</p>
                    <button onclick="openPay()" class="w-full gold-bg text-black text-[10px] font-black py-3 rounded-full uppercase tracking-tighter">Buy Order</button>
                </div>`;
        }
    </script>
</body>
</html>
