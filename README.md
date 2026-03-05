<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>अयांश कृषि मुनीम | Final V38</title>
    <style>
        :root { --p: #1b5e20; --s: #ff8f00; --d: #b71c1c; --w: #ffffff; --g: #f1f8e9; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: var(--g); margin: 0; padding-bottom: 80px; color: #333; }
        
        /* लॉगिन स्क्रीन */
        #passScreen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--p); z-index: 9999; display: flex; align-items: center; justify-content: center; }
        .pass-box { background: white; padding: 30px; border-radius: 20px; width: 85%; max-width: 320px; text-align: center; box-shadow: 0 10px 25px rgba(0,0,0,0.3); }
        #passInp { width: 100%; padding: 12px; font-size: 22px; text-align: center; border: 2px solid #ddd; border-radius: 10px; margin: 15px 0; outline: none; }

        .header { background: var(--p); color: white; padding: 15px; text-align: center; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .page { display: none; padding: 10px; max-width: 500px; margin: auto; }
        .page-active { display: block; animation: fadeIn 0.3s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .card { background: var(--w); padding: 15px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.06); margin-bottom: 12px; border: 1px solid #e0e0e0; }
        h3 { margin: 0 0 10px 0; color: var(--p); font-size: 16px; border-bottom: 1px solid #eee; padding-bottom: 5px; }
        
        input, select { width: 100%; padding: 10px; margin-top: 5px; border: 1.5px solid #ccc; border-radius: 8px; box-sizing: border-box; font-size: 14px; }
        label { font-size: 12px; font-weight: bold; color: #666; display: block; margin-top: 8px; }

        .btn { border: none; padding: 12px; border-radius: 8px; width: 100%; font-weight: bold; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; margin-top: 10px; font-size: 14px; }
        .btn-p { background: var(--p); color: white; }
        .btn-s { background: var(--s); color: white; }
        .btn-pay { background: #2e7d32; color: white; }
        .btn-del { background: #ffebee; color: var(--d); padding: 5px 10px; width: auto; font-size: 11px; }

        table { width: 100%; border-collapse: collapse; margin-top: 10px; font-size: 13px; }
        th, td { border: 1px solid #eee; padding: 8px; text-align: left; }
        th { background: #f5f5f5; }

        .nav-bar { display: flex; background: white; position: fixed; bottom: 0; width: 100%; height: 65px; border-top: 1px solid #ddd; box-shadow: 0 -2px 10px rgba(0,0,0,0.05); }
        .nav-item { flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; color: #777; font-size: 12px; cursor: pointer; }
        .nav-active { color: var(--p); font-weight: bold; background: #f9fff9; border-top: 3px solid var(--p); }

        /* हिस्ट्री स्टाइल */
        .hist-row { border-bottom: 1px solid #eee; padding: 10px 0; display: flex; justify-content: space-between; align-items: center; }
        .tag { font-size: 10px; padding: 2px 6px; border-radius: 4px; text-transform: uppercase; }
        .tag-bill { background: #fff3e0; color: #e65100; }
        .tag-pay { background: #e8f5e9; color: #2e7d32; }
    </style>
</head>
<body>

<div id="passScreen">
    <div class="pass-box">
        <h2 style="margin:0; color:var(--p);">अयांश मुनीम</h2>
        <p style="font-size:12px; color:#666;">सुरक्षित लॉगिन पिन डालें</p>
        <input type="password" id="passInp" maxlength="4" placeholder="****">
        <button class="btn btn-p" onclick="checkLogin()">प्रवेश करें</button>
    </div>
</div>

<div id="appBody" style="display:none;">
    <div class="header"><h2 style="margin:0; font-size:18px;">अयांश कृषि मुनीम</h2></div>

    <div id="p-main" class="page page-active">
        <div class="card">
            <h3>👤 किसान की पहचान</h3>
            <label>किसान का नाम:</label>
            <input type="text" id="cN" placeholder="नाम लिखें..." list="custNames">
            <datalist id="custNames"></datalist>
            <label>मोबाइल (वैकल्पिक):</label>
            <input type="number" id="cM" placeholder="91XXXXXXXX">
        </div>

        <div class="card">
            <h3>🛒 सामान जोड़ें</h3>
            <input type="text" id="iN" placeholder="सामान का नाम" list="stockList">
            <datalist id="stockList"></datalist>
            <div style="display:flex; gap:8px;">
                <input type="number" id="iP" placeholder="दर (₹)">
                <input type="number" id="iQ" placeholder="मात्रा">
            </div>
            <button class="btn btn-p" onclick="addToTemp()">➕ लिस्ट में डालें</button>
        </div>

        <div id="tempBillBox" class="card" style="display:none;">
            <h3>📄 कच्चा बिल</h3>
            <table id="tempTable">
                <thead><tr><th>नाम</th><th>टोटल</th><th>हटाएं</th></tr></thead>
                <tbody></tbody>
            </table>
            <div style="text-align:right; margin-top:10px;">
                <strong>कुल योग: <span id="tempTotal" style="color:var(--d); font-size:20px;">₹0</span></strong>
            </div>
            <button class="btn btn-s" onclick="finalizeBill()">💾 खाता में सुरक्षित करें</button>
        </div>
    </div>

    <div id="p-khata" class="page">
        <div class="card">
            <h3>📜 किसान लेजर (बकाया सूची)</h3>
            <input type="text" id="srch" placeholder="किसान का नाम खोजें..." onkeyup="renderLedger()">
            <div id="ledgerList" style="margin-top:15px;"></div>
        </div>
    </div>

    <div id="p-stock" class="page">
        <div class="card">
            <h3>📦 स्टॉक सेटिंग</h3>
            <input type="text" id="sN" placeholder="सामान का नाम">
            <input type="number" id="sP" placeholder="फिक्स रेट ₹">
            <button class="btn btn-p" onclick="addStock()">स्टॉक में जोड़ें</button>
        </div>
        <div class="card">
            <table id="stockTable">
                <thead><tr><th>नाम</th><th>रेट</th><th>X</th></tr></thead>
                <tbody></tbody>
            </table>
        </div>
    </div>

    <nav class="nav-bar">
        <div class="nav-item nav-active" onclick="tab('main', this)">🛒<span>बिल</span></div>
        <div class="nav-item" onclick="tab('khata', this)">📜<span>खाता</span></div>
        <div class="nav-item" onclick="tab('stock', this)">📦<span>स्टॉक</span></div>
    </nav>
</div>

<div id="modal" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.8); z-index:2000; display:none; align-items:center; justify-content:center;">
    <div class="card" style="width:90%; max-height:85%; overflow-y:auto; border-top:5px solid var(--p);">
        <h2 id="modalTitle" style="margin:0; color:var(--p);">किसान का नाम</h2>
        <p id="modalSub" style="font-size:12px; color:#666;"></p>
        
        <div id="modalHistory" style="margin:15px 0; border:1px solid #eee; padding:10px; border-radius:8px; max-height:200px; overflow-y:auto; background:#fafafa;"></div>
        
        <div style="background:#fff3e0; padding:10px; border-radius:8px;">
            <label>पैसे जमा करें (Payment Entry):</label>
            <input type="number" id="payInp" placeholder="₹ रसीद राशि">
            <button class="btn btn-pay" onclick="savePayment()">✅ जमा करें</button>
        </div>
        <button class="btn" style="background:#eee; color:#333;" onclick="closeModal()">वापस जाएं</button>
    </div>
</div>

<script>
    // डेटा स्ट्रक्चर
    let DB = {
        stk: JSON.parse(localStorage.getItem('ay_stk') || '[]'),
        tx: JSON.parse(localStorage.getItem('ay_tx') || '[]')
    };

    let tempBill = [];
    let currentCust = "";

    function checkLogin() {
        if(document.getElementById('passInp').value === "1234") {
            document.getElementById('passScreen').style.display = 'none';
            document.getElementById('appBody').style.display = 'block';
            syncUI();
        } else { alert("गलत पिन!"); }
    }

    function syncUI() {
        renderStock();
        updateCustDatalist();
    }

    function tab(p, el) {
        document.querySelectorAll('.page').forEach(pg => pg.classList.remove('page-active'));
        document.getElementById('p-'+p).classList.add('page-active');
        document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('nav-active'));
        el.classList.add('nav-active');
        if(p === 'khata') renderLedger();
    }

    // स्टॉक फंक्शन
    function addStock() {
        let n = document.getElementById('sN').value.trim();
        let p = document.getElementById('sP').value;
        if(!n || !p) return;
        DB.stk.push({n, p: parseFloat(p)});
        save(); renderStock();
        document.getElementById('sN').value = ""; document.getElementById('sP').value = "";
    }

    function renderStock() {
        let h = ""; let opts = "";
        DB.stk.forEach((s, i) => {
            h += `<tr><td>${s.n}</td><td>₹${s.p}</td><td onclick="removeStock(${i})">❌</td></tr>`;
            opts += `<option value="${s.n}">`;
        });
        document.querySelector('#stockTable tbody').innerHTML = h;
        document.getElementById('stockList').innerHTML = opts;
    }

    function removeStock(i) { DB.stk.splice(i, 1); save(); renderStock(); }

    // बिलिंग फंक्शन
    document.getElementById('iN').addEventListener('input', (e) => {
        let match = DB.stk.find(s => s.n === e.target.value);
        if(match) document.getElementById('iP').value = match.p;
    });

    function addToTemp() {
        let n = document.getElementById('iN').value.trim();
        let p = parseFloat(document.getElementById('iP').value);
        let q = parseFloat(document.getElementById('iQ').value);
        if(!n || !p || !q) return alert("विवरण पूरा भरें");
        
        tempBill.push({n, p, q, t: p * q});
        renderTemp();
        document.getElementById('iN').value = ""; document.getElementById('iP').value = ""; document.getElementById('iQ').value = "";
    }

    function renderTemp() {
        let h = ""; let total = 0;
        tempBill.forEach((item, i) => {
            h += `<tr><td>${item.n} (${item.q})</td><td>₹${item.t}</td><td onclick="tempBill.splice(${i},1);renderTemp()">❌</td></tr>`;
            total += item.t;
        });
        document.querySelector('#tempTable tbody').innerHTML = h;
        document.getElementById('tempTotal').innerText = "₹" + total;
        document.getElementById('tempBillBox').style.display = tempBill.length ? 'block' : 'none';
    }

    function finalizeBill() {
        let cn = document.getElementById('cN').value.trim();
        let cm = document.getElementById('cM').value;
        if(!cn) return alert("किसान का नाम जरूरी है");

        let total = tempBill.reduce((s, i) => s + i.t, 0);
        DB.tx.push({
            id: Date.now(),
            name: cn,
            mobile: cm,
            amount: total,
            type: 'bill',
            date: new Date().toLocaleDateString('hi-IN'),
            items: [...tempBill]
        });

        save();
        alert("बिल सुरक्षित किया गया");
        tempBill = [];
        renderTemp();
        document.getElementById('cN').value = ""; document.getElementById('cM').value = "";
        updateCustDatalist();
    }

    // लेजर (खाता) फंक्शन
    function renderLedger() {
        let q = document.getElementById('srch').value.toLowerCase();
        let ledger = {};

        DB.tx.forEach(t => {
            if(!ledger[t.name]) ledger[t.name] = { bal: 0, mobile: t.mobile };
            ledger[t.name].bal += (t.type === 'bill' ? t.amount : -t.amount);
        });

        let h = "";
        for(let name in ledger) {
            if(name.toLowerCase().includes(q)) {
                let b = ledger[name].bal;
                h += `<div class="card" onclick="openDetails('${name}')" style="display:flex; justify-content:space-between; align-items:center; border-left:5px solid ${b>0?'var(--d)':'#2e7d32'}">
                    <div><strong>${name}</strong><br><small>${ledger[name].mobile || 'कोई नंबर नहीं'}</small></div>
                    <div style="text-align:right">
                        <span style="font-size:11px; color:#666;">बकाया</span><br>
                        <strong style="color:${b>0?'var(--d)':'#2e7d32'}">₹${b}</strong>
                    </div>
                </div>`;
            }
        }
        document.getElementById('ledgerList').innerHTML = h || "कोई रिकॉर्ड नहीं मिला";
    }

    function openDetails(name) {
        currentCust = name;
        document.getElementById('modalTitle').innerText = name;
        let hist = DB.tx.filter(t => t.name === name);
        let bal = hist.reduce((s, t) => s + (t.type === 'bill' ? t.amount : -t.amount), 0);
        
        document.getElementById('modalSub').innerText = "कुल बकाया: ₹" + bal;
        
        let hHtml = hist.map((t, idx) => `
            <div class="hist-row">
                <div>
                    <span class="tag ${t.type==='bill'?'tag-bill':'tag-pay'}">${t.type==='bill'?'उधार':'जमा'}</span>
                    <br><small style="color:#888">${t.date}</small>
                </div>
                <div style="font-weight:bold; color:${t.type==='bill'?'var(--d)':'var(--p)'}">
                    ${t.type==='bill'?'+':'-'}₹${t.amount}
                </div>
                <button class="btn-del" onclick="deleteTx(${t.id})">हटाएं</button>
            </div>
        `).reverse().join('');

        document.getElementById('modalHistory').innerHTML = hHtml || "कोई हिस्ट्री नहीं है";
        document.getElementById('modal').style.display = 'flex';
    }

    function savePayment() {
        let amt = parseFloat(document.getElementById('payInp').value);
        if(!amt) return alert("रकम लिखें");
        
        DB.tx.push({
            id: Date.now(),
            name: currentCust,
            amount: amt,
            type: 'pay',
            date: new Date().toLocaleDateString('hi-IN')
        });
        
        save();
        document.getElementById('payInp').value = "";
        openDetails(currentCust); // रिफ्रेश हिस्ट्री
    }

    function deleteTx(id) {
        if(confirm("क्या आप इस एंट्री को हटाना चाहते हैं?")) {
            DB.tx = DB.tx.filter(t => t.id !== id);
            save();
            openDetails(currentCust);
        }
    }

    function closeModal() { 
        document.getElementById('modal').style.display = 'none'; 
        renderLedger();
    }

    function updateCustDatalist() {
        let names = [...new Set(DB.tx.map(t => t.name))];
        document.getElementById('custNames').innerHTML = names.map(n => `<option value="${n}">`).join('');
    }

    function save() {
        localStorage.setItem('ay_stk', JSON.stringify(DB.stk));
        localStorage.setItem('ay_tx', JSON.stringify(DB.tx));
    }
</script>
</body>
</html>
