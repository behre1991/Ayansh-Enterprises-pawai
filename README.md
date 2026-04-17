<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Shop Heyar-Sheyar</title>
    <style>
        :root { --primary: #0d47a1; --bg: #f4f7f9; --white: #ffffff; }
        body { font-family: 'Segoe UI', Arial, sans-serif; margin: 0; background: var(--bg); padding-bottom: 80px; overflow-x: hidden; }
        
        /* पिन लॉक स्क्रीन */
        #lockScreen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--primary); color: white; z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        
        /* हेडर और प्रोफाइल */
        .header { background: var(--primary); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; }
        .sidebar { position: fixed; top: 0; left: -100%; width: 85%; height: 100%; background: white; transition: 0.4s; z-index: 2000; overflow-y: auto; box-shadow: 5px 0 15px rgba(0,0,0,0.3); }
        .sidebar.open { left: 0; }
        .menu-point { padding: 15px; border-bottom: 1px solid #eee; font-weight: 600; cursor: pointer; color: #333; }
        
        /* कार्ड और इनपुट */
        .container { padding: 15px; }
        .card { background: var(--white); padding: 20px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); margin-bottom: 15px; }
        input, select, textarea { width: 100%; padding: 12px; margin: 10px 0; border: 1px solid #ccc; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        .btn-main { background: var(--primary); color: white; border: none; padding: 15px; border-radius: 10px; width: 100%; font-size: 17px; font-weight: bold; cursor: pointer; }
        .plus-btn { background: #2e7d32; color: white; border: none; width: 45px; height: 45px; border-radius: 50%; font-size: 28px; cursor: pointer; float: right; }

        /* बॉटम नेविगेशन */
        .nav-bar { position: fixed; bottom: 0; width: 100%; background: white; display: flex; border-top: 3px solid var(--primary); height: 70px; z-index: 1000; }
        .nav-item { flex: 1; text-align: center; padding: 12px 0; font-size: 12px; cursor: pointer; color: #666; font-weight: bold; }
        .nav-item.active { color: var(--primary); background: #e3f2fd; }
        
        .screen { display: none; }
        .screen.active { display: block; }
    </style>
</head>
<body onload="initApp()">

    <div id="lockScreen">
        <h2>Smart Shop Heyar-Sheyar 🦁</h2>
        <p>सुरक्षित प्रवेश के लिए पिन डालें</p>
        <input type="password" id="pinInput" placeholder="PIN दर्ज करें" style="width: 60%; text-align: center; color: black;">
        <button class="btn-main" onclick="verifyPin()" style="width: 60%; background: #ffd700; color: black;">खोलें</button>
    </div>

    <div class="header">
        <div onclick="toggleMenu()" style="display:flex; align-items:center; gap:10px; cursor:pointer;">
            <div style="background:white; color:var(--primary); padding:5px 10px; border-radius:5px; font-weight:bold;">SS</div>
            <span style="font-size: 18px;">स्मार्ट शॉप, शेर, शेर</span>
        </div>
        <div onclick="startVoice()" style="font-size: 24px; cursor:pointer;">🎤</div>
    </div>

    <div id="sideMenu" class="sidebar">
        <div style="background:var(--primary); color:white; padding:30px 20px;"><h3>मास्टर सेटिंग्स</h3></div>
        <div class="menu-point" onclick="alert('शॉप सेटिंग: नाम और पता बदलें')">1. ⚙️ शॉप सेटिंग</div>
        <div class="menu-point" onclick="alert('भाषा चयन: हिंदी/मराठी')">2. 🌐 भाषा & वॉइस व्यवस्था</div>
        <div class="menu-point" onclick="showDateReport()">3. 📊 रिपोर्ट मास्टर (तारीख से तारीख)</div>
        <div class="menu-point" onclick="alert('विभाग मास्टर: खाद/बीज/दवाई जोड़ें')">4. 📁 विभाग मास्टर (+)</div>
        <div class="menu-point" onclick="changeAppPin()">5. 🔐 सुरक्षा, पेमेंट (QR/PIN/UPI)</div>
        <div class="menu-point" onclick="alert('सुधार व्यवस्था चालू है')">6. ✏️ सुधार सिस्टम (Edit/Delete)</div>
        <div class="menu-point" onclick="manageStaff()">7. 👥 स्टाफ मैनेजमेंट (आधार/सैलरी)</div>
        <div class="menu-point" onclick="alert('हेल्पलाइन: 1800-XXX-XXXX')">8. 🆘 सपोर्ट & हेल्पलाइन</div>
        <button onclick="toggleMenu()" class="btn-main" style="background:#444; margin:20px; width:88%;">वापस जाएँ</button>
    </div>

    <div class="container">
        
        <div id="billingScreen" class="screen active">
            <div class="card">
                <button class="plus-btn" onclick="clearBill()">+</button>
                <h3>स्मार्ट बिलिंग</h3>
                <input type="text" id="custName" placeholder="ग्राहक का नाम">
                <input type="number" id="custMob" placeholder="मोबाइल नंबर">
                <input type="date" id="billDate">
                <hr>
                <input type="text" id="itemName" placeholder="सामान का नाम (ऑटो-सर्च)">
                <div style="display:flex; gap:10px;">
                    <input type="number" id="itemRate" placeholder="रेट">
                    <input type="number" id="itemQty" placeholder="मात्रा">
                </div>
                <textarea id="details" placeholder="दवाई विवरण (यहाँ लिखें)..."></textarea>
                <button class="btn-main" onclick="generateBill()">शेयर बिल (QR सहित) 📤</button>
            </div>
        </div>

        <div id="khataScreen" class="screen">
            <div class="card"><h3>लेजर खाता</h3><p>ग्राहक और सप्लायर का बकाया यहाँ दिखेगा।</p></div>
        </div>

        <div id="stockScreen" class="screen">
            <div class="card">
                <h3>स्टॉक मैनेजमेंट</h3>
                <button class="btn-main" style="background:#2e7d32;" onclick="alert('PDF/Print तैयार हो रहा है...')">स्टॉक PDF शेयर करें</button>
            </div>
        </div>

        <div id="inwardScreen" class="screen">
            <div class="card">
                <h3>नया माल (आवक)</h3>
                <input type="text" placeholder="सप्लायर का नाम">
                <input type="number" placeholder="सप्लायर GST">
                <input type="number" placeholder="खरीद रेट">
                <input type="number" placeholder="बिक्री रेट">
                <button class="btn-main">माल स्टॉक में जोड़ें</button>
            </div>
        </div>

        <div id="expScreen" class="screen">
            <div class="card"><h3>दुकान खर्च & स्टाफ</h3><p>चाय, बिजली और स्टाफ सैलरी यहाँ मैनेज करें।</p></div>
        </div>

    </div>

    <div class="nav-bar">
        <div class="nav-item active" onclick="switchNav('billingScreen', this)">📄<br>बिलिंग</div>
        <div class="nav-item" onclick="switchNav('khataScreen', this)">📖<br>खाता</div>
        <div class="nav-item" onclick="switchNav('stockScreen', this)">📦<br>स्टॉक</div>
        <div class="nav-item" onclick="switchNav('inwardScreen', this)">🚚<br>आवक</div>
        <div class="nav-item" onclick="switchNav('expScreen', this)">💰<br>दुकान खर्च</div>
    </div>

    <script>
        let myPin = "1234"; // शुरुआती पिन

        function initApp() {
            document.getElementById('billDate').valueAsDate = new Date();
        }

        // सुरक्षा फंक्शन
        function verifyPin() {
            const input = document.getElementById('pinInput').value;
            if(input === myPin) {
                document.getElementById('lockScreen').style.display = 'none';
            } else {
                alert("गलत पिन! सही पिन डालें।");
            }
        }

        function changeAppPin() {
            let p = prompt("नया सुरक्षा PIN सेट करें:");
            if(p && p.length >= 4) {
                myPin = p;
                alert("पिन सफलतापूर्वक बदल गया है!");
            }
        }

        // नेविगेशन फंक्शन
        function switchNav(screenId, element) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
            element.classList.add('active');
        }

        function toggleMenu() {
            document.getElementById('sideMenu').classList.toggle('open');
        }

        // बिलिंग फंक्शन्स
        function clearBill() {
            document.getElementById('custName').value = '';
            document.getElementById('custMob').value = '';
            document.getElementById('itemName').value = '';
            document.getElementById('itemRate').value = '';
            document.getElementById('itemQty').value = '';
            document.getElementById('details').value = '';
            alert("मैजिक प्लस (+): स्क्रीन साफ़ कर दी गई है!");
        }

        function generateBill() {
            alert("Smart Shop Heyar-Sheyar: बिल, QR कोड और UPI (नाम सहित) व्हाट्सएप पर भेजा जा रहा है...");
        }

        function showDateReport() {
            let start = prompt("शुरुआत तारीख (DD/MM/YYYY):");
            let end = prompt("अंतिम तारीख (DD/MM/YYYY):");
            if(start && end) alert(start + " से " + end + " की रिपोर्ट तैयार है!");
        }

        function startVoice() {
            alert("वॉइस कमांड एक्टिव है... बोलिए!");
        }

        function manageStaff() {
            alert("स्टाफ: फोटो, आधार [Redacted], सैलरी और जॉइनिंग डेट की जानकारी यहाँ सुरक्षित है।");
        }
    </script>
</body>
</html>
