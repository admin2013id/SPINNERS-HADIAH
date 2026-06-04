<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEXUS.ID - VIP Spinner</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #8b5cf6;
            --secondary: #ec4899;
            --gold: #fbbf24;
            --dark-bg: #0f172a;
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
            --text-main: #ffffff;
            --text-muted: #94a3b8;
            --danger: #ef4444;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }

        body {
            background-color: var(--dark-bg);
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(139, 92, 246, 0.15) 0%, transparent 40%),
                radial-gradient(circle at 90% 80%, rgba(236, 72, 153, 0.15) 0%, transparent 40%);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
        }

        .container { width: 100%; max-width: 480px; padding: 20px; position: relative; }

        /* Glassmorphism Card */
        .card {
            background: var(--glass-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .brand-logo {
            font-size: 2rem; font-weight: 800; text-align: center; margin-bottom: 10px;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
            letter-spacing: 2px;
        }

        input {
            width: 100%; padding: 15px; margin-bottom: 15px; border-radius: 12px;
            border: 1px solid var(--glass-border); background: rgba(0, 0, 0, 0.2);
            color: white; font-size: 1rem; outline: none; transition: all 0.3s;
        }
        input:focus { border-color: var(--primary); box-shadow: 0 0 15px rgba(139, 92, 246, 0.3); }

        .btn {
            width: 100%; padding: 15px; border: none; border-radius: 12px;
            font-size: 1rem; font-weight: 600; cursor: pointer; transition: all 0.3s;
            text-transform: uppercase; letter-spacing: 1px; margin-bottom: 10px;
        }
        .btn-primary {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white; box-shadow: 0 4px 15px rgba(139, 92, 246, 0.4);
        }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(139, 92, 246, 0.6); }
        
        .btn-gold {
            background: linear-gradient(135deg, #f59e0b, #d97706);
            color: black; box-shadow: 0 4px 15px rgba(251, 191, 36, 0.4);
        }
        .btn-outline {
            background: transparent; border: 1px solid var(--glass-border);
            color: var(--text-muted);
        }
        .btn-danger {
            background: rgba(239, 68, 68, 0.1);
            border: 1px solid var(--danger);
            color: var(--danger);
        }
        .btn-danger:hover { background: var(--danger); color: white; }

        .hidden { display: none !important; }

        /* Spinner */
        .wheel-container { position: relative; width: 280px; height: 280px; margin: 0 auto 30px; }
        .wheel {
            width: 100%; height: 100%; border-radius: 50%;
            border: 8px solid var(--glass-border);
            background: conic-gradient(from 0deg, #8b5cf6 0deg 90deg, #ec4899 90deg 180deg, #fbbf24 180deg 270deg, #3b82f6 270deg 360deg);
            box-shadow: 0 0 30px rgba(139, 92, 246, 0.2);
            transition: transform 4s cubic-bezier(0.17, 0.67, 0.12, 0.99);
            position: relative;
        }
        .wheel::after {
            content: "NEXUS"; position: absolute; top: 50%; left: 50%;
            transform: translate(-50%, -50%); background: var(--dark-bg);
            width: 60px; height: 60px; border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            font-weight: bold; border: 2px solid var(--gold); color: var(--gold); z-index: 2;
        }
        .pointer {
            position: absolute; top: -15px; left: 50%; transform: translateX(-50%);
            width: 0; height: 0; border-left: 15px solid transparent;
            border-right: 15px solid transparent; border-top: 25px solid white;
            z-index: 10; filter: drop-shadow(0 2px 4px rgba(0,0,0,0.5));
        }

        /* Token & History */
        .token-display {
            background: rgba(0, 0, 0, 0.3); padding: 15px; border-radius: 15px;
            text-align: center; margin-bottom: 20px; border: 1px solid var(--gold);
        }
        .token-count { font-size: 1.5rem; color: var(--gold); font-weight: bold; }
        
        .history-list { max-height: 150px; overflow-y: auto; margin-top: 10px; }
        .history-item {
            background: rgba(255, 255, 255, 0.05); padding: 10px; margin-bottom: 8px;
            border-radius: 8px; font-size: 0.9rem; display: flex; justify-content: space-between;
        }

        /* Modal Toko */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.8); backdrop-filter: blur(5px);
            z-index: 100; display: flex; justify-content: center; align-items: center;
        }
        .shop-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px; }
        .shop-item {
            background: rgba(255, 255, 255, 0.05); padding: 15px; border-radius: 12px;
            text-align: center; border: 1px solid var(--glass-border); cursor: pointer; transition: 0.3s;
        }
        .shop-item:hover { border-color: var(--primary); background: rgba(139, 92, 246, 0.1); }
        .shop-price { color: var(--gold); font-size: 0.9rem; margin-top: 5px; }
        
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-thumb { background: var(--glass-border); border-radius: 3px; }

        .footer-links {
            margin-top: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .report-btn {
            background: rgba(59, 130, 246, 0.1);
            color: #60a5fa;
            border: 1px solid rgba(59, 130, 246, 0.3);
            padding: 10px;
            border-radius: 8px;
            text-align: center;
            text-decoration: none;
            font-size: 0.9rem;
            display: block;
        }
        .report-btn:hover { background: rgba(59, 130, 246, 0.2); }
    </style>
</head>
<body>

    <div class="container">
        <!-- Step 1: Input Nama / Login -->
        <div id="step-name" class="card">
            <div class="brand-logo">NEXUS.ID <span style="font-size:0.8rem; background:var(--gold); color:black; padding:2px 5px; border-radius:4px;">VIP</span></div>
            <p style="text-align: center; color: var(--text-muted); margin-bottom: 20px;">Platform Hadiah Digital Terpercaya</p>
            
            <label style="color: var(--text-muted); font-size: 0.9rem;">Masukkan Nama Akun (Login/Register)</label>
            <input type="text" id="nama" placeholder="Contoh: ProGamer123">
            <button class="btn btn-primary" onclick="lanjutKeOTP()">Masuk / Daftar</button>
            
            <div style="margin-top: 15px; text-align: center; font-size: 0.8rem; color: var(--text-muted);">
                <p>Belum punya akun? Masukkan nama baru untuk mendaftar.</p>
                <p>Sudah punya akun? Masukkan nama yang sama untuk login.</p>
            </div>
        </div>

        <!-- Step 2: OTP -->
        <div id="step-otp" class="card hidden">
            <h2>Verifikasi Keamanan</h2>
            <p style="text-align: center; color: var(--text-muted); margin-bottom: 20px;">
                Kode 6 digit telah dikirim.<br><small>(Cek Alert/Console untuk kode)</small>
            </p>
            <input type="number" id="otp" placeholder="Masukkan 6 Digit Kode">
            <button class="btn btn-primary" onclick="verifikasiOTP()">Verifikasi Sekarang</button>
            <button class="btn btn-outline" onclick="kirimUlangOTP()">Kirim Ulang Kode</button>
            <button class="btn btn-outline" onclick="kembaliKeLogin()" style="margin-top: 5px;">&larr; Kembali</button>
        </div>

        <!-- Main App -->
        <div id="main-app" class="card hidden">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                <div class="brand-logo" style="font-size: 1.5rem; margin:0;">NEXUS.ID</div>
                <div style="text-align: right;">
                    <div style="font-size: 0.8rem; color: var(--text-muted);">Halo,</div>
                    <div id="displayNama" style="font-weight: bold; color: var(--primary);">User</div>
                </div>
            </div>

            <!-- Token Section -->
            <div class="token-display">
                <div style="font-size: 0.9rem; color: var(--text-muted);">Saldo Token Anda</div>
                <div class="token-count" id="tokenCount">0</div>
                <button class="btn btn-outline" style="padding: 5px 10px; font-size: 0.8rem; margin-top: 5px;" onclick="showToko()">
                    🛒 Toko Penukaran
                </button>
            </div>

            <!-- Spinner Section -->
            <div class="wheel-container">
                <div class="pointer"></div>
                <div id="wheel" class="wheel"></div>
            </div>

            <button id="spinBtn" class="btn btn-primary" onclick="putarSpinner()">SPIN SEKARANG</button>

            <!-- Claim Code Section -->
            <div style="margin-top: 30px; border-top: 1px solid var(--glass-border); padding-top: 20px;">
                <h3 style="font-size: 1.1rem; margin-bottom: 10px;">🎁 Claim Code VIP</h3>
                <div style="display: flex; gap: 10px;">
                    <input type="text" id="claimCode" placeholder="Masukkan Code" style="margin-bottom: 0;">
                    <button class="btn btn-gold" style="width: auto; padding: 0 20px;" onclick="claimCodeNexus()">CLAIM</button>
                </div>
                <p id="claimStatus" style="font-size: 0.8rem; margin-top: 5px; text-align: center;"></p>
            </div>

            <!-- History Section -->
            <div style="margin-top: 20px;">
                <h3 style="font-size: 1rem; margin-bottom: 10px;">📜 Riwayat Hadiah</h3>
                <div id="historyList" class="history-list">
                    <div style="text-align: center; color: var(--text-muted); font-size: 0.8rem;">Belum ada riwayat</div>
                </div>
            </div>

            <!-- Footer & Report Bug -->
            <div class="footer-links">
                <a href="https://wa.me/6285882382854?text=Halo%20Admin%20Nexus,%20saya%20ingin%20report%20bug/kehilangan%20akun." target="_blank" class="report-btn">
                    📢 Report Bug / Kehilangan Akun
                </a>
                
                <button class="btn btn-danger" onclick="logoutAkun()">
                    🚪 Log Out Akun
                </button>
                
                <div style="text-align: center; font-size: 0.8rem; color: var(--text-muted); margin-top: 10px;">
                    <p>Butuh Bantuan? Hubungi Admin</p>
                    <p style="color: var(--primary); font-weight: bold;">WA: 0858-8238-2854</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Toko -->
    <div id="tokoModal" class="modal-overlay hidden">
        <div class="card" style="width: 90%; max-width: 400px;">
            <div style="display: flex; justify-content: space-between; align-items: center;">
                <h2 style="margin: 0;">Toko Penukaran</h2>
                <button onclick="closeToko()" style="background: none; border: none; color: white; font-size: 1.5rem; cursor: pointer;">&times;</button>
            </div>
            <div class="shop-grid">
                <!-- Format M (Juta) -->
                <div class="shop-item" onclick="tukarHadiah('AKUN FF', 1000000)">
                    <div style="font-weight: bold;">AKUN FF</div>
                    <div class="shop-price">1M Token</div>
                </div>
                <div class="shop-item" onclick="tukarHadiah('AKUN ML', 5000000)">
                    <div style="font-weight: bold;">AKUN ML</div>
                    <div class="shop-price">5M Token</div>
                </div>
                <div class="shop-item" onclick="tukarHadiah('AKUN PUBG', 100000000)">
                    <div style="font-weight: bold;">AKUN PUBG</div>
                    <div class="shop-price">100M Token</div>
                </div>
                <!-- Pengecualian Random (Harga Tetap/JT) -->
                <div class="shop-item" onclick="tukarHadiah('AKUN RANDOM', 5000000)">
                    <div style="font-weight: bold;">AKUN RANDOM</div>
                    <div class="shop-price">5JT Token</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // --- KONFIGURASI WIPES (RESET DATA) ---
        // Tanggal Reset: 4 Juni 2026, Jam 12:00:00
        const WIPE_DATE = new Date('2026-06-04T12:00:00'); 

        // --- DATA USER ---
        let allUsers = {};
        let currentUser = null;
        // Bonus Akun Baru: 50JT
        const DEFAULT_TOKEN = 50000000; 
        
        let userData = { nama: "", token: DEFAULT_TOKEN, history: [], claimedCodes: [] };
        let otpBenar = "";

        // --- SISTEM RESET OTOMATIS ---
        function checkAndWipeData() {
            const now = new Date();
            const lastWipe = localStorage.getItem('nexusLastWipe');
            
            // Jika waktu sekarang MELEBIHI tanggal wipe, DAN (belum pernah wipe ATAU last wipe lebih lama dari tanggal wipe)
            if (now > WIPE_DATE && (!lastWipe || new Date(lastWipe) < WIPE_DATE)) {
                console.log("Waktunya Reset Data! Menghapus semua akun...");
                
                // Hapus semua data user
                localStorage.removeItem('nexusUsers');
                localStorage.removeItem('nexusCurrentUser');
                localStorage.removeItem('nexusUser'); // Format lama
                
                // Tandai bahwa wipe sudah dilakukan
                localStorage.setItem('nexusLastWipe', now.toISOString());
                
                alert("⚠️ SYSTEM RESET ⚠️\n\nSemua data akun telah direset pada jadwal yang ditentukan (4 Juni 12:00).\nSilakan daftar akun baru untuk mendapatkan bonus 50JT Token!");
                
                // Force reload untuk membersihkan state memori
                location.reload();
            }
        }

        // --- FUNGSI LOAD & SAVE ---
        function loadAllUsers() {
            const saved = localStorage.getItem('nexusUsers');
            if (saved) {
                allUsers = JSON.parse(saved);
            }
        }

        function saveAllUsers() {
            localStorage.setItem('nexusUsers', JSON.stringify(allUsers));
        }

        function saveCurrentUser() {
            if (currentUser) {
                allUsers[currentUser] = userData;
                saveAllUsers();
                localStorage.setItem('nexusCurrentUser', currentUser);
            }
        }

        function loadData(nama) {
            loadAllUsers();
            if (allUsers[nama]) {
                userData = allUsers[nama];
                currentUser = nama;
            } else {
                // User Baru: Bonus 50JT
                userData = { nama: nama, token: DEFAULT_TOKEN, history: [], claimedCodes: [] };
                currentUser = nama;
                allUsers[nama] = userData;
                saveAllUsers();
                alert(`🎉 Selamat Datang ${nama}!\nAnda mendapatkan bonus pendaftaran: 50.000.000 Token!`);
            }
            
            document.getElementById('displayNama').textContent = userData.nama;
            updateTokenDisplay();
            updateHistory();
        }

        function updateTokenDisplay() { document.getElementById('tokenCount').textContent = userData.token.toLocaleString('id-ID'); }
        
        function updateHistory() {
            const list = document.getElementById('historyList');
            if (userData.history.length === 0) {
                list.innerHTML = '<div style="text-align: center; color: var(--text-muted); font-size: 0.8rem;">Belum ada riwayat</div>';
                return;
            }
            list.innerHTML = userData.history.map(item => `
                <div class="history-item">
                    <span style="color: var(--gold);">${item.hadiah}</span>
                    <span style="color: var(--text-muted); font-size: 0.8rem;">${item.tanggal}</span>
                </div>
            `).join('');
        }

        // --- LOGIN & OTP ---
        function lanjutKeOTP() {
            const nama = document.getElementById('nama').value.trim();
            if (!nama) return alert("Masukkan nama Anda!");
            
            sessionStorage.setItem('tempNama', nama);
            
            otpBenar = Math.floor(100000 + Math.random() * 900000).toString();
            alert(`Kode OTP Simulasi: ${otpBenar}`);
            
            document.getElementById('step-name').classList.add('hidden');
            document.getElementById('step-otp').classList.remove('hidden');
        }

        function verifikasiOTP() {
            const inputOTP = document.getElementById('otp').value;
            const tempNama = sessionStorage.getItem('tempNama');
            
            if (inputOTP === otpBenar) {
                loadData(tempNama);
                
                document.getElementById('step-otp').classList.add('hidden');
                document.getElementById('main-app').classList.remove('hidden');
            } else { 
                alert("Kode OTP salah!"); 
            }
        }

        function kirimUlangOTP() {
            otpBenar = Math.floor(100000 + Math.random() * 900000).toString();
            alert(`Kode baru: ${otpBenar}`);
        }

        function kembaliKeLogin() {
            document.getElementById('step-otp').classList.add('hidden');
            document.getElementById('step-name').classList.remove('hidden');
        }

        // --- LOGOUT ---
        function logoutAkun() {
            if(confirm("Apakah Anda yakin ingin Log Out?")) {
                currentUser = null;
                userData = { nama: "", token: DEFAULT_TOKEN, history: [], claimedCodes: [] };
                localStorage.removeItem('nexusCurrentUser');
                
                document.getElementById('main-app').classList.add('hidden');
                document.getElementById('step-name').classList.remove('hidden');
                document.getElementById('nama').value = "";
                document.getElementById('claimCode').value = "";
                document.getElementById('claimStatus').textContent = "";
            }
        }

        // --- SPINNER (PROBABILITAS BARU) ---
        const prizes = [ 
            {name: "ZONK / COBA LAGI", prob: 55}, 
            {name: "AKUN FF", prob: 10}, 
            {name: "AKUN ML", prob: 10}, 
            {name: "AKUN RANDOM", prob: 15}, 
            {name: "AKUN PUBG", prob: 10} 
        ];
        
        function putarSpinner() {
            if (!currentUser) return alert("Silakan login terlebih dahulu!");
            
            const btn = document.getElementById('spinBtn');
            btn.disabled = true; btn.textContent = "SPINNING...";
            const wheel = document.getElementById('wheel');
            const rotation = 1800 + Math.floor(Math.random() * 360); 
            wheel.style.transform = `rotate(${rotation}deg)`;
            
            setTimeout(() => {
                const hadiah = getRandomPrize();
                userData.history.unshift({ hadiah: hadiah, tanggal: new Date().toLocaleTimeString('id-ID') });
                if(userData.history.length > 10) userData.history.pop();
                
                saveCurrentUser();
                updateHistory();
                
                alert(hadiah !== "ZONK / COBA LAGI" ? `🎉 SELAMAT! Anda mendapatkan: ${hadiah}` : `😢 Yahh, ${hadiah}. Coba lagi besok!`);
                btn.disabled = false; btn.textContent = "SPIN SEKARANG";
            }, 4000);
        }

        function getRandomPrize() {
            const rand = Math.random() * 100; let sum = 0;
            for (let prize of prizes) { sum += prize.prob; if (rand <= sum) return prize.name; }
            return "ZONK / COBA LAGI";
        }

        // --- CLAIM CODE & NOTIFIKASI EMAIL (WEB3FORMS) ---
        function claimCodeNexus() {
            if (!currentUser) return alert("Silakan login terlebih dahulu!");

            const codeInput = document.getElementById('claimCode');
            const statusText = document.getElementById('claimStatus');
            const codeRaw = codeInput.value.trim();
            const code = codeRaw.toUpperCase();
            const now = new Date().toLocaleString('id-ID');

            if (!code) { statusText.textContent = "Masukkan code terlebih dahulu."; statusText.style.color = "red"; return; }

            // Cek sekali pakai
            if (userData.claimedCodes.includes(code)) {
                statusText.textContent = "Code ini sudah Anda gunakan sebelumnya!";
                statusText.style.color = "orange";
                alert("⚠️ Code ini hanya bisa digunakan sekali per akun!");
                return;
            }

            // Validasi Code
            let reward = 0;
            let isValid = false;

            if (code === "GO.NEXUS") {
                reward = 25000000; // 25JT Token
                isValid = true;
            } else if (code === "NEXUS.GAMESID") {
                reward = 5000000; // 5JT Token
                isValid = true;
            } else if (code.includes("NEXUS")) {
                reward = 1000000; // 1JT Token (Code Lama)
                isValid = true;
            }

            if (isValid) {
                // Update Data User
                userData.token += reward;
                userData.claimedCodes.push(code);
                saveCurrentUser();
                updateTokenDisplay();
                
                // Kirim Notifikasi Email via Web3Forms
                kirimNotifikasiEmail(userData.nama, code, now, reward);

                statusText.textContent = `✅ Berhasil! +${reward.toLocaleString()} Token`;
                statusText.style.color = "#4ade80";
                codeInput.value = "";
                alert(`✅ Claim Berhasil!\n+${reward.toLocaleString()} Token telah ditambahkan.\nNotifikasi telah dikirim ke Admin.`);
            } else {
                statusText.textContent = "❌ Code tidak valid.";
                statusText.style.color = "red";
                alert("Code Salah!");
            }
        }

        function kirimNotifikasiEmail(nama, code, waktu, reward) {
            const accessKey = "5a58df98-c319-4aa1-a9ea-2ae4117eca4d"; 
            
            const messageBody = `
NOTIFIKASI CLAIM CODE BARU
------------------------------
NAMA : ${nama}
CODE : ${code}
WAKTU : ${waktu}
REWARD : ${reward.toLocaleString()} Token
------------------------------
            `;

            const formData = new FormData();
            formData.append("access_key", accessKey);
            formData.append("subject", `Claim Code Baru: ${code} oleh ${nama}`);
            formData.append("from_name", "NEXUS.ID System");
            formData.append("message", messageBody);

            fetch("https://api.web3forms.com/submit", {
                method: "POST",
                body: formData
            })
            .then(async (response) => {
                let json = await response.json();
                if (response.status == 200) {
                    console.log("Email berhasil dikirim ke Admin:", json);
                } else {
                    console.log("Gagal mengirim email:", json.message);
                }
            })
            .catch(error => {
                console.log(error);
            });
        }

        // --- TOKO ---
        function showToko() { 
            if (!currentUser) return alert("Silakan login terlebih dahulu!");
            document.getElementById('tokoModal').classList.remove('hidden'); 
        }
        function closeToko() { document.getElementById('tokoModal').classList.add('hidden'); }
        
        function tukarHadiah(item, harga) {
            if (userData.token >= harga) {
                if(confirm(`Tukar ${item} seharga ${harga.toLocaleString()} Token?`)) {
                    userData.token -= harga;
                    userData.history.unshift({ hadiah: `BELI ${item}`, tanggal: new Date().toLocaleString('id-ID') });
                    saveCurrentUser();
                    updateTokenDisplay(); updateHistory();
                    alert(`✅ Sukses membeli ${item}!`); closeToko();
                }
            } else { alert("❌ Token tidak cukup!"); }
        }

        // Init Check Session & Wipe
        window.onload = function() {
            // 1. Cek apakah perlu reset data
            checkAndWipeData();
            
            // 2. Load session jika ada
            const savedCurrent = localStorage.getItem('nexusCurrentUser');
            if (savedCurrent) {
                loadAllUsers(); // Load data users yang tersisa (jika tidak di-wipe)
                if (allUsers[savedCurrent]) {
                    loadData(savedCurrent);
                    document.getElementById('step-name').classList.add('hidden');
                    document.getElementById('main-app').classList.remove('hidden');
                }
            }
        };
    </script>
</body>
</html>
