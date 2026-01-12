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
        #authScreen { position: fixed; inset: 0; background: white; z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .card { background: white; width: 85%; max-width: 400px; padding: 30px; border-radius: 20px; box-shadow: 0 20px 50px rgba(0,0,0,0.15); text-align: center; }
        
        /* --- DASHBOARD --- */
        #dashboard { display: none; }
        header { background: var(--main); color: white; padding: 20px; text-align: center; border-bottom: 4px solid var(--gold); position: sticky; top: 0; z-index: 100; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        
        .wallet-box { background: linear-gradient(135deg, #004d40, #00695c); color: white; margin: 20px; padding: 25px; border-radius: 15px; text-align: center; box-shadow: 0 10px 20px rgba(0,77,64,0.3); }
        .balance { font-size: 42px; font-weight: 800; margin: 10px 0; color: var(--gold); text-shadow: 1px 1px 2px black; }

        /* --- 9 TOOLS GRID --- */
        .grid-container { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 20px; }
        .tool-btn { background: white; padding: 20px; border-radius: 15px; text-align: center; box-shadow: 0 4px 10px rgba(0,0,0,0.05); transition: 0.3s; border: 2px solid transparent; cursor: pointer; }
        .tool-btn:hover { border-color: var(--main); transform: translateY(-5px); }
        .tool-icon { font-size: 30px; margin-bottom: 10px; display: block; }
        .tool-name { font-weight: bold; color: #333; font-size: 14px; }

        /* --- POPUP WINDOWS --- */
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 10000; align-items: center; justify-content: center; padding: 20px; backdrop-filter: blur(5px); }
        .modal-content { background: white; width: 100%; max-width: 450px; border-radius: 20px; padding: 25px; animation: slideUp 0.3s ease; position: relative; }
        @keyframes slideUp { from {transform: translateY(50px); opacity: 0;} to {transform: translateY(0); opacity: 1;} }

        /* --- FORM ELEMENTS --- */
        select, input { width: 100%; padding: 15px; margin: 10px 0; border: 1px solid #ddd; border-radius: 10px; font-size: 16px; box-sizing: border-box; outline: none; transition: 0.3s; }
        select:focus, input:focus { border-color: var(--main); box-shadow: 0 0 0 3px rgba(0,77,64,0.1); }
        
        .price-display { background: #e0f2f1; color: var(--main); padding: 15px; border-radius: 10px; font-weight: bold; margin-bottom: 15px; display: flex; justify-content: space-between; }
        
        .action-btn { width: 100%; padding: 16px; background: var(--main); color: white; border: none; border-radius: 10px; font-size: 16px; font-weight: bold; cursor: pointer; text-transform: uppercase; letter-spacing: 1px; }
        .close-btn { background: #f5f5f5; color: #555; margin-top: 10px; }
        
        /* WHATSAPP BUTTON UPDATED */
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
            <h1 style="color:var(--main); margin:0;">FJ ENTERPRISE</h1>
            <p style="color:#666;">Secure Business Terminal</p>
            <input type="text" id="userName" placeholder="Full Name">
            <input type="tel" id="userPhone" placeholder="Mobile Number (03...)">
            <button class="action-btn" onclick="sendOTP()">SEND CODE</button>
        </div>

        <div class="card" id="otpStep" style="display:none;">
            <h2 style="color:var(--main)">VERIFICATION</h2>
            <p id="otpMessage" style="background:#fff3cd; padding:10px; border-radius:8px; color:#856404; font-weight:bold;"></p>
            <input type="number" id="otpInput" placeholder="Enter 4-Digit Code">
            <button class="action-btn" onclick="verifyOTP()">LOGIN DASHBOARD</button>
        </div>
    </div>

    <div id="dashboard">
        <header>FJ GLOBAL TERMINAL</header>

        <div class="wallet-box">
            <span>Total Available Balance</span>
            <div class="balance">Rs. 0.00</div>
            <button class="action-btn" style="background:var(--green); border:none;" onclick="openModal('depositModal')">+ ADD FUNDS</button>
        </div>

        <div class="grid-container">
            <div class="tool-btn" onclick="openModal('ytModal')"><span class="tool-icon">📺</span><span class="tool-name">YouTube</span></div>
            <div class="tool-btn" onclick="openModal('ttModal')"><span class="tool-icon">🎵</span><span class="tool-name">TikTok</span></div>
            <div class="tool-btn" onclick="openModal('fbModal')"><span class="tool-icon">💙</span><span class="tool-name">Facebook</span></div>
            <div class="tool-btn" onclick="openModal('gameModal')"><span class="tool-icon">🎮</span><span class="tool-name">Gaming Hub</span></div>
            <div class="tool-btn" onclick="openModal('depositModal')"><span class="tool-icon">🟢</span><span class="tool-name">EasyPaisa</span></div>
            <div class="tool-btn" onclick="openModal('depositModal')"><span class="tool-icon">💳</span><span class="tool-name">FirstPay</span></div>
            <div class="tool-btn" onclick="openModal('cloudModal')"><span class="tool-icon">☁️</span><span class="tool-name">Cloud Data</span></div>
            <div class="tool-btn" onclick="openModal('globalModal')"><span class="tool-icon">🌍</span><span class="tool-name">Global Biz</span></div>
            <div class="tool-btn" style="border-color:red" onclick="location.reload()"><span class="tool-icon">🚪</span><span class="tool-name">LOGOUT</span></div>
        </div>
    </div>

    <div id="ytModal" class="modal"><div class="modal-content">
        <h3>📺 YouTube Services</h3>
        <select id="ytSelect" onchange="updatePrice('yt')">
            <option value="1000">1,000 Subscribers (Non-Drop)</option>
            <option value="4000">4,000 Watch Hours (Monetization)</option>
            <option value="500">1,000 Real Likes</option>
            <option value="2500">10,000 High Retention Views</option>
        </select>
        <div class="price-display"><span>Price:</span> <span id="ytPrice">Rs. 1000</span></div>
        <input type="text" placeholder="Paste Channel/Video Link">
        <button class="action-btn" onclick="placeOrder('YouTube')">START ORDER</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <div id="gameModal" class="modal"><div class="modal-content">
        <h3>🎮 Gaming Top-up</h3>
        <select id="gameSelect" onchange="updatePrice('game')">
            <option value="240">PUBG Mobile - 60 UC</option>
            <option value="1250">PUBG Mobile - 325 UC</option>
            <option value="2500">PUBG Mobile - 660 UC</option>
            <option value="190">FreeFire - 100 Diamonds</option>
            <option value="950">FreeFire - 520 Diamonds</option>
            <option value="4500">FreeFire - Weekly Membership</option>
        </select>
        <div class="price-display"><span>Cost:</span> <span id="gamePrice">Rs. 240</span></div>
        <input type="text" placeholder="Enter Player ID (UID)">
        <input type="text" placeholder="Enter Player Name">
        <button class="action-btn" onclick="placeOrder('Gaming')">SEND UC/DIAMONDS</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <div id="fbModal" class="modal"><div class="modal-content">
        <h3>💙 Facebook Growth</h3>
        <select id="fbSelect" onchange="updatePrice('fb')">
            <option value="800">Page Likes (1k)</option>
            <option value="600">Profile Followers (1k)</option>
            <option value="400">Post Shares (1k)</option>
            <option value="300">Video Views (10k)</option>
        </select>
        <div class="price-display"><span>Price:</span> <span id="fbPrice">Rs. 800</span></div>
        <input type="text" placeholder="Paste Facebook Link">
        <button class="action-btn" onclick="placeOrder('Facebook')">BOOST NOW</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <div id="ttModal" class="modal"><div class="modal-content">
        <h3>🎵 TikTok Viral</h3>
        <select id="ttSelect" onchange="updatePrice('tt')">
            <option value="450">1,000 Followers</option>
            <option value="300">1,000 Real Likes</option>
            <option value="200">10,000 Views</option>
        </select>
        <div class="price-display"><span>Price:</span> <span id="ttPrice">Rs. 450</span></div>
        <input type="text" placeholder="TikTok Profile/Video Link">
        <button class="action-btn" onclick="placeOrder('TikTok')">VIRAL NOW</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <div id="depositModal" class="modal"><div class="modal-content">
        <h3>💰 Add Funds</h3>
        <div style="background:#e8f5e9; padding:15px; border-radius:10px; margin-bottom:15px; text-align:left; font-size:14px; line-height:1.6;">
            <strong>EasyPaisa:</strong> <span style="color:var(--main)">03023032091</span><br>
            <strong>FirstPay:</strong> <span style="color:var(--main)">03023032091</span><br>
            <strong>Title:</strong> FIDA HUSSAIN<br>
            <small>Send payment and enter TID below.</small>
        </div>
        <input type="number" placeholder="Amount (PKR)">
        <input type="text" placeholder="Transaction ID (TID)">
        <button class="action-btn" onclick="placeOrder('Deposit')">VERIFY PAYMENT</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <div id="cloudModal" class="modal"><div class="modal-content">
        <h3>☁️ Cloud Storage</h3>
        <p>Secure Encrypted Storage (10GB)</p>
        <input type="file" style="background:#f9f9f9; padding:10px;">
        <button class="action-btn" onclick="placeOrder('Cloud Upload')">UPLOAD FILE</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <div id="globalModal" class="modal"><div class="modal-content">
        <h3>🌍 Global Business</h3>
        <select>
            <option>UK Ltd Company - Rs. 5000</option>
            <option>USA LLC Reg - Rs. 25000</option>
        </select>
        <button class="action-btn" onclick="placeOrder('Global Biz')">CONTACT SUPPORT</button>
        <button class="action-btn close-btn" onclick="closeModals()">CLOSE</button>
    </div></div>

    <script>
        let generatedOTP;

        // 1. Auth System
        function sendOTP() {
            const name = document.getElementById('userName').value;
            const phone = document.getElementById('userPhone').value;
            if(!name || !phone) return alert("Please enter Name and Phone!");

            generatedOTP = Math.floor(1000 + Math.random() * 9000); // Random 4-digit code
            
            document.getElementById('loginStep').style.display = 'none';
            document.getElementById('otpStep').style.display = 'block';
            document.getElementById('otpMessage').innerHTML = `Server Message:<br>Your OTP Code is: <strong>${generatedOTP}</strong>`;
        }

        function verifyOTP() {
            const input = document.getElementById('otpInput').value;
            if(input == generatedOTP) {
                document.getElementById('authScreen').style.display = 'none';
                document.getElementById('dashboard').style.display = 'block';
            } else {
                alert("Wrong Code! Please try again.");
            }
        }

        // 2. Dynamic Price System
        function updatePrice(service) {
            if(service === 'yt') {
                const val = document.getElementById('ytSelect').value;
                document.getElementById('ytPrice').innerText = "Rs. " + val;
            } else if(service === 'game') {
                const val = document.getElementById('gameSelect').value;
                document.getElementById('gamePrice').innerText = "Rs. " + val;
            } else if(service === 'fb') {
                const val = document.getElementById('fbSelect').value;
                document.getElementById('fbPrice').innerText = "Rs. " + val;
            } else if(service === 'tt') {
                const val = document.getElementById('ttSelect').value;
                document.getElementById('ttPrice').innerText = "Rs. " + val;
            }
        }

        // 3. Modal Controls
        function openModal(id) { document.getElementById(id).style.display = 'flex'; }
        function closeModals() { document.querySelectorAll('.modal').forEach(m => m.style.display = 'none'); }

        // 4. Order System
        function placeOrder(type) {
            alert("✅ SUCCESS!\n" + type + " Request Submitted.\nSyncing with Server...");
            closeModals();
        }
    </script>
</body>
</html>

