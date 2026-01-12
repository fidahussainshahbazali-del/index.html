<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FJ GLOBAL | World Class Terminal</title>
    <style>
        :root { --main: #004d40; --green: #00c853; --gold: #ffd700; --bg: #f4f7f6; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); margin: 0; user-select: none; }

        /* --- AUTH SCREEN --- */
        #authScreen { position: fixed; inset: 0; background: white; z-index: 9999; display: flex; align-items: center; justify-content: center; }
        .card { background: white; width: 85%; max-width: 400px; padding: 30px; border-radius: 20px; box-shadow: 0 20px 50px rgba(0,0,0,0.15); text-align: center; }
        
        /* --- DASHBOARD --- */
        #dashboard { display: none; }
        header { background: var(--main); color: white; padding: 20px; text-align: center; border-bottom: 4px solid var(--gold); font-weight: bold; position: sticky; top: 0; z-index: 100; }
        
        .wallet-box { background: linear-gradient(135deg, #004d40, #00695c); color: white; margin: 15px; padding: 25px; border-radius: 15px; text-align: center; box-shadow: 0 10px 20px rgba(0,0,0,0.1); position: relative; }
        .balance { font-size: 42px; font-weight: 800; margin: 10px 0; color: var(--gold); }
        .admin-trigger { position: absolute; top: 5px; right: 10px; font-size: 10px; opacity: 0.4; cursor: pointer; color: white; }

        /* --- 9 TOOLS GRID --- */
        .grid-container { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 15px; margin-bottom: 100px; }
        .tool-btn { background: white; padding: 20px; border-radius: 15px; text-align: center; box-shadow: 0 4px 10px rgba(0,0,0,0.05); transition: 0.3s; border: 2px solid transparent; cursor: pointer; }
        .tool-btn:active { transform: scale(0.95); background: #f0f0f0; }
        .tool-icon { font-size: 35px; margin-bottom: 8px; display: block; }
        .tool-name { font-weight: bold; color: #333; font-size: 14px; }

        /* --- POPUP WINDOWS --- */
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 10000; align-items: center; justify-content: center; padding: 20px; backdrop-filter: blur(5px); }
        .modal-content { background: white; width: 100%; max-width: 450px; border-radius: 20px; padding: 25px; position: relative; max-height: 80vh; overflow-y: auto; }

        input, select { width: 100%; padding: 15px; margin: 10px 0; border: 1px solid #ddd; border-radius: 10px; font-size: 16px; box-sizing: border-box; outline: none; }
        .action-btn { width: 100%; padding: 16px; background: var(--main); color: white; border: none; border-radius: 10px; font-size: 16px; font-weight: bold; cursor: pointer; }
        .close-btn { background: #eee; color: #555; margin-top: 10px; }
        
        /* WhatsApp Button (03443744818) */
        .whatsapp-float { position: fixed; bottom: 25px; left: 25px; cursor: pointer; z-index: 200; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
    </style>
</head>
<body>

    <div class="whatsapp-float" onclick="window.open('https://wa.me/923443744818')">
        <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" width="60" alt="WA">
    </div>

    <div id="authScreen">
        <div class="card" id="loginStep">
            <h1 style="color:var(--main)">FJ REGISTER</h1>
            <p>Secure Business Node</p>
            <input type="text" id="regName" placeholder="Full Name">
            <input type="tel" id="regPhone" placeholder="Mobile Number">
            <button class="action-btn" onclick="sendOTP()">SEND OTP CODE</button>
        </div>
        <div class="card" id="otpStep" style="display:none;">
            <h2>VERIFY OTP</h2>
            <p id="otpMsg" style="background:#fff3cd; padding:10px; font-weight:bold; border-radius:8px;"></p>
            <input type="number" id="otpIn" placeholder="Enter 4-Digit Code">
            <button class="action-btn" onclick="completeLogin()">START DASHBOARD</button>
        </div>
    </div>

    <div id="dashboard">
        <header>FJ GLOBAL TERMINAL</header>
        <div class="wallet-box">
            <div class="admin-trigger" onclick="adminControl()">SERVER ID: 2B87AR 🔒</div>
            <div id="userNameTxt" style="font-size: 14px; opacity: 0.9;">Welcome</div>
            <div id="userBal" class="balance">Rs. 0.00</div>
            <button class="action-btn" style="background:var(--green); width:auto; padding:10px 30px;" onclick="openModal('depModal')">ADD FUNDS</button>
        </div>

        <div class="grid-container">
            <div class="tool-btn" onclick="openModal('ytModal')"><span class="tool-icon">📺</span><span class="tool-name">YouTube</span></div>
            <div class="tool-btn" onclick="openModal('ttModal')"><span class="tool-icon">🎵</span><span class="tool-name">TikTok</span></div>
            <div class="tool-btn" onclick="openModal('fbModal')"><span class="tool-icon">💙</span><span class="tool-name">Facebook</span></div>
            <div class="tool-btn" onclick="openModal('gmModal')"><span class="tool-icon">🎮</span><span class="tool-name">Gaming Hub</span></div>
            <div class="tool-btn" onclick="openModal('depModal')"><span class="tool-icon">🟢</span><span class="tool-name">EasyPaisa</span></div>
            <div class="tool-btn" onclick="openModal('depModal')"><span class="tool-icon">💳</span><span class="tool-name">FirstPay</span></div>
            <div class="tool-btn" onclick="openModal('clModal')"><span class="tool-icon">☁️</span><span class="tool-name">Cloud Data</span></div>
            <div class="tool-btn" onclick="openModal('gbModal')"><span class="tool-icon">🌍</span><span class="tool-name">Global Biz</span></div>
            <div class="tool-btn" style="border-color:red" onclick="logout()"><span class="tool-icon">🚪</span><span class="tool-name" style="color:red">Logout</span></div>
        </div>
    </div>

    <div id="ytModal" class="modal"><div class="modal-content">
        <h3>YouTube Services</h3>
        <select id="ytType"><option value="1000">1k Subscribers (Rs. 1000)</option><option value="4000">4k WatchTime (Rs. 4000)</option><option value="500">1k Likes (Rs. 500)</option></select>
        <input type="text" placeholder="Channel/Video URL">
        <button class="action-btn" onclick="placeOrder('YouTube', document.getElementById('ytType').value)">PLACE ORDER</button>
        <button class="action-btn close-btn" onclick="closeModal()">CLOSE</button>
    </div></div>

    <div id="ttModal" class="modal"><div class="modal-content">
        <h3>TikTok Viral</h3>
        <select id="ttType"><option value="450">1k Followers (Rs. 450)</option><option value="300">1k Likes (Rs. 300)</option></select>
        <input type="text" placeholder="Video Link">
        <button class="action-btn" onclick="placeOrder('TikTok', document.getElementById('ttType').value)">PLACE ORDER</button>
        <button class="action-btn close-btn" onclick="closeModal()">CLOSE</button>
    </div></div>

    <div id="fbModal" class="modal"><div class="modal-content">
        <h3>Facebook Growth</h3>
        <select id="fbType"><option value="800">1k Page Likes (Rs. 800)</option><option value="600">1k Followers (Rs. 600)</option></select>
        <input type="text" placeholder="Facebook Link">
        <button class="action-btn" onclick="placeOrder('Facebook', document.getElementById('fbType').value)">PLACE ORDER</button>
        <button class="action-btn close-btn" onclick="closeModal()">CLOSE</button>
    </div></div>

    <div id="gmModal" class="modal"><div class="modal-content">
        <h3>Gaming Top-Up</h3>
        <select id="gmType">
            <option value="240">PUBG 60 UC (Rs. 240)</option>
            <option value="1250">PUBG 325 UC (Rs. 1250)</option>
            <option value="190">FF 100 Gems (Rs. 190)</option>
            <option value="950">FF 530 Gems (Rs. 950)</option>
        </select>
        <input type="text" placeholder="Player UID">
        <button class="action-btn" onclick="placeOrder('Gaming', document.getElementById('gmType').value)">BUY NOW</button>
        <button class="action-btn close-btn" onclick="closeModal()">CLOSE</button>
    </div></div>

    <div id="depModal" class="modal"><div class="modal-content">
        <h3>Add Money (PKR)</h3>
        <div style="background:#e8f5e9; padding:15px; border-radius:10px; font-size:14px; margin-bottom:10px;">
            <b>EasyPaisa/FirstPay:</b> 03023032091 <br>
            <b>Title:</b> FIDA HUSSAIN
        </div>
        <input type="number" id="dAmt" placeholder="Amount Sent">
        <input type="text" id="dTid" placeholder="Transaction TID">
        <button class="action-btn" style="background:var(--green)" onclick="manualDeposit()">SUBMIT FOR APPROVAL</button>
        <button class="action-btn close-btn" onclick="closeModal()">CANCEL</button>
    </div></div>

    <div id="clModal" class="modal"><div class="modal-content"><h3>Cloud Storage</h3><input type="file"><button class="action-btn" onclick="alert('Saved to Secure Key!')">UPLOAD</button><button class="action-btn close-btn" onclick="closeModal()">CLOSE</button></div></div>
    <div id="gbModal" class="modal"><div class="modal-content"><h3>Global Biz</h3><button class="action-btn">CONSULT FJ TEAM</button><button class="action-btn close-btn" onclick="closeModal()">CLOSE</button></div></div>

    <script>
        // --- MASTER DATA KEY ---
        const MASTER_KEY = "AhZ6bG1-ZmlkYS1odXNzYWluLTJiODdhchYLEgpEYXRhIHNhdmUgIgZFcmFpbmcM";
        let tempOTP;

        // Auto-Load on Refresh
        window.onload = function() {
            const data = localStorage.getItem(MASTER_KEY);
            if(data) {
                showDashboard(JSON.parse(data));
            }
        };

        // 1. Auth System
        function sendOTP() {
            const name = document.getElementById('regName').value;
            const phone = document.getElementById('regPhone').value;
            if(!name || !phone) return alert("Please fill all details!");

            tempOTP = Math.floor(1000 + Math.random() * 9000);
            document.getElementById('loginStep').style.display = 'none';
            document.getElementById('otpStep').style.display = 'block';
            document.getElementById('otpMsg').innerText = "Server OTP: " + tempOTP;
        }

        function completeLogin() {
            if(document.getElementById('otpIn').value == tempOTP) {
                const user = {
                    name: document.getElementById('regName').value,
                    phone: document.getElementById('regPhone').value,
                    balance: 0
                };
                localStorage.setItem(MASTER_KEY, JSON.stringify(user));
                showDashboard(user);
            } else { alert("Wrong OTP Code!"); }
        }

        function showDashboard(user) {
            document.getElementById('authScreen').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';
            document.getElementById('userNameTxt').innerText = "Account: " + user.name;
            updateBalanceUI(user.balance);
        }

        function updateBalanceUI(b) {
            document.getElementById('userBal').innerText = "Rs. " + b.toFixed(2);
        }

        // 2. Manual Deposit Logic (No Auto-Add)
        function manualDeposit() {
            const amt = document.getElementById('dAmt').value;
            const tid = document.getElementById('dTid').value;
            const name = JSON.parse(localStorage.getItem(MASTER_KEY)).name;
            
            if(!amt || !tid) return alert("Enter Amount & TID");

            const msg = `DEPOSIT ALERT:\nUser: ${name}\nAmount: Rs. ${amt}\nTID: ${tid}`;
            window.open(`https://wa.me/923443744818?text=${encodeURIComponent(msg)}`);
            
            alert("✅ Request Sent!\nBalance will be added manually after Fida Hussain verifies the payment.");
            closeModal();
        }

        // 3. Admin Control (How you add money)
        function adminControl() {
            let pass = prompt("Enter Admin Password:");
            if(pass === "fida786") {
                let amount = parseInt(prompt("How much balance to add?"));
                if(!isNaN(amount)) {
                    let user = JSON.parse(localStorage.getItem(MASTER_KEY));
                    user.balance += amount;
                    localStorage.setItem(MASTER_KEY, JSON.stringify(user));
                    updateBalanceUI(user.balance);
                    alert("✅ Balance Added Successfully!");
                }
            } else { alert("Access Denied!"); }
        }

        // 4. Order Logic
        function placeOrder(type, cost) {
            cost = parseInt(cost);
            let user = JSON.parse(localStorage.getItem(MASTER_KEY));
            
            if(user.balance >= cost) {
                user.balance -= cost;
                localStorage.setItem(MASTER_KEY, JSON.stringify(user));
                updateBalanceUI(user.balance);
                alert("🚀 Order Placed Successfully for " + type);
                closeModal();
            } else {
                alert("❌ Low Balance! Please contact Fida Hussain for deposit.");
            }
        }

        // Helpers
        function openModal(id) { document.getElementById(id).style.display = 'flex'; }
        function closeModal() { document.querySelectorAll('.modal').forEach(m => m.style.display = 'none'); }
        function logout() { localStorage.removeItem(MASTER_KEY); location.reload(); }
    </script>
</body>
</html>
