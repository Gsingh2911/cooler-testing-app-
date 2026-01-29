<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Motor Testing V4 (Universal Scanner)</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>

    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; background-color: #eef2f6; padding: 10px; max-width: 800px; margin: 0 auto; }
        h2 { text-align: center; color: #0d47a1; margin-bottom: 10px; }
        
        .card { background: white; padding: 15px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin-bottom: 15px; }
        
        .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .full-width { grid-column: span 2; }
        
        label { display: block; margin-top: 8px; font-weight: bold; font-size: 13px; color: #444; }
        input, select { width: 100%; padding: 12px; margin-top: 4px; border: 1px solid #ccc; border-radius: 6px; box-sizing: border-box; font-size: 16px; }
        
        /* Calculated Fields */
        .calculated { background-color: #e3f2fd; font-weight: bold; color: #0d47a1; border: 1px solid #90caf9; pointer-events: none; }

        /* Buttons */
        button { width: 100%; padding: 14px; margin-top: 15px; border: none; border-radius: 8px; font-size: 16px; font-weight: bold; cursor: pointer; }
        .btn-save { background-color: #0d47a1; color: white; }
        .btn-scan { background-color: #ffca28; color: black; width: 100%; margin-top: 23px; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-download { background-color: #2e7d32; color: white; }
        .btn-clear { background-color: #c62828; color: white; margin-top: 10px; }

        /* Scanner Box */
        #reader { width: 100%; margin-bottom: 15px; display: none; border-radius: 8px; overflow: hidden; border: 5px solid #333; }
        
        /* Table */
        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 12px; white-space: nowrap; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: center; }
        th { background: #f5f5f5; }

        /* QR Result */
        #qr-container { text-align: center; margin-top: 10px; padding: 15px; background: #fff; display: none; border: 2px dashed #ccc; }
    </style>
</head>
<body>

    <h2>Motor Test Log V4</h2>

    <div class="card">
        <div id="reader"></div>

        <div class="form-grid">
            <div><label>Date</label><input type="date" id="testDate"></div>
            <div><label>Technician</label><input type="text" id="techName" placeholder="Name"></div>

            <div style="grid-column: span 1;">
                <label>Serial Number</label>
                <input type="text" id="serialNo" placeholder="Scan or Type...">
            </div>
            <div style="grid-column: span 1;">
                <button type="button" class="btn-scan" onclick="startScanner()">
                    📷 SCAN
                </button>
            </div>

            <div class="full-width">
                <label>Motor Type</label>
                <select id="motorType">
                    <option value="" disabled selected>-- Select Motor --</option>
                    <option value="Kit motor">Kit motor</option>
                    <option value="110motor 32mm">110motor 32mm</option>
                    <option value="110 motor 28mm">110 motor 28mm</option>
                    <option value="Mini kit">Mini kit</option>
                    <option value="Exhaust motor">Exhaust motor</option>
                    <option value="H frame motor 32mm">H frame motor 32mm</option>
                    <option value="H frame motor 38mm">H frame motor 38mm</option>
                    <option value="H frame motor 44mm">H frame motor 44mm</option>
                    <option value="H frame motor 55mm">H frame motor 55mm</option>
                </select>
            </div>

            <div><label>FPM 1</label><input type="number" class="calc-input" id="fpm1"></div>
            <div><label>FPM 2</label><input type="number" class="calc-input" id="fpm2"></div>
            <div><label>FPM 3</label><input type="number" class="calc-input" id="fpm3"></div>
            <div><label>FPM 4</label><input type="number" class="calc-input" id="fpm4"></div>
            <div><label>FPM 5</label><input type="number" class="calc-input" id="fpm5"></div>
            
            <div>
                <label>Multiplier</label>
                <select id="multiplier" class="calc-input">
                    <option value="0.44">0.44</option>
                    <option value="0.79">0.79</option>
                    <option value="1.40">1.40</option>
                    <option value="1.77">1.77</option>
                    <option value="2.64">2.64</option>
                    <option value="3.69">3.69</option>
                    <option value="4.91">4.91</option>
                </select>
            </div>

            <div><label>Avg Reading</label><input type="text" id="avgReading" class="calculated" readonly></div>
            <div><label>FINAL CFM</label><input type="text" id="finalCFM" class="calculated" readonly></div>

            <div>
                <label>Pump Status</label>
                <select id="pumpStatus">
                    <option value="OKAY">OKAY</option>
                    <option value="NOT OKAY">NOT OKAY</option>
                    <option value="N/A">N/A</option>
                </select>
            </div>
            <div>
                <label>Auto Swing</label>
                <select id="swingStatus">
                    <option value="OKAY">OKAY</option>
                    <option value="NOT OKAY">NOT OKAY</option>
                    <option value="N/A">N/A</option>
                </select>
            </div>

            <div class="full-width">
                <label>Remark</label>
                <input type="text" id="remark" placeholder="Remarks...">
            </div>
        </div>

        <button class="btn-save" onclick="saveLog()">✅ SAVE & GENERATE QR</button>
        
        <div id="qr-container">
            <h3 style="margin:0;">Entry Saved!</h3>
            <p style="font-size:12px; color:#666;">QR contains Serial, CFM & Status</p>
            <div id="qrcode" style="display: inline-block; margin-top:10px;"></div>
        </div>
    </div>

    <div class="card">
        <h3>Log History (<span id="count">0</span>)</h3>
        <div class="table-container">
            <table id="logTable">
                <thead><tr><th>Serial</th><th>Motor</th><th>CFM</th><th>Result</th></tr></thead>
                <tbody></tbody>
            </table>
        </div>
        <button class="btn-download" onclick="downloadCSV()">📥 Download Excel</button>
        <button class="btn-clear" onclick="clearData()">🗑️ Reset</button>
    </div>

    <script>
        // --- 1. CONFIGURATION FOR SCANNER ---
        let html5QrCode;
        
        function startScanner() {
            const readerDiv = document.getElementById("reader");
            readerDiv.style.display = "block";
            
            html5QrCode = new Html5Qrcode("reader");

            // This config enables BOTH Barcodes and QR Codes
            const config = { 
                fps: 10, 
                qrbox: { width: 250, height: 250 },
                aspectRatio: 1.0
            };

            // START SCANNING
            html5QrCode.start(
                { facingMode: "environment" }, 
                config,
                (decodedText) => {
                    // SUCCESS: We found a code!
                    document.getElementById('serialNo').value = decodedText;
                    
                    // Stop camera immediately
                    html5QrCode.stop().then(() => {
                        readerDiv.style.display = "none";
                    });
                },
                (errorMessage) => { 
                    // Scanning... (ignore errors while searching)
                }
            ).catch(err => {
                alert("CAMERA ERROR: " + err + "\n\nNOTE: You MUST host this on GitHub/Netlify (HTTPS). Camera does not work on local files.");
                readerDiv.style.display = "none";
            });
        }

        // --- 2. CALCULATIONS ---
        const inputs = document.querySelectorAll('.calc-input');
        inputs.forEach(input => input.addEventListener('input', calculate));
        inputs.forEach(input => input.addEventListener('change', calculate));

        function calculate() {
            const v1 = parseFloat(document.getElementById('fpm1').value) || 0;
            const v2 = parseFloat(document.getElementById('fpm2').value) || 0;
            const v3 = parseFloat(document.getElementById('fpm3').value) || 0;
            const v4 = parseFloat(document.getElementById('fpm4').value) || 0;
            const v5 = parseFloat(document.getElementById('fpm5').value) || 0;
            const multi = parseFloat(document.getElementById('multiplier').value) || 0;

            const avg = (v1 + v2 + v3 + v4 + v5) / 5;
            const cfm = avg * multi;

            document.getElementById('avgReading').value = avg > 0 ? avg.toFixed(2) : '';
            document.getElementById('finalCFM').value = cfm > 0 ? cfm.toFixed(2) : '';
        }

        // --- 3. SAVE & GENERATE QR ---
        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('testDate').valueAsDate = new Date();
            renderTable();
        });

        function saveLog() {
            const data = {
                date: document.getElementById('testDate').value,
                tech: document.getElementById('techName').value || "N/A",
                serial: document.getElementById('serialNo').value,
                motor: document.getElementById('motorType').value || "N/A",
                fpm: [
                    document.getElementById('fpm1').value || 0,
                    document.getElementById('fpm2').value || 0,
                    document.getElementById('fpm3').value || 0,
                    document.getElementById('fpm4').value || 0,
                    document.getElementById('fpm5').value || 0
                ].join(' | '),
                multi: document.getElementById('multiplier').value,
                avg: document.getElementById('avgReading').value,
                cfm: document.getElementById('finalCFM').value,
                pump: document.getElementById('pumpStatus').value,
                swing: document.getElementById('swingStatus').value,
                remark: document.getElementById('remark').value
            };

            if(!data.serial) { alert("⚠️ Scan or Enter Serial Number first!"); return; }

            // Save to Database (Local)
            let logs = JSON.parse(localStorage.getItem('motorLogsV4')) || [];
            logs.unshift(data);
            localStorage.setItem('motorLogsV4', JSON.stringify(logs));

            // Generate Output QR
            generateOutputQR(data);
            renderTable();
            
            // Clear inputs (Smart clear)
            document.getElementById('serialNo').value = "";
            document.getElementById('fpm1').value = ""; 
            document.getElementById('fpm2').value = "";
            document.getElementById('fpm3').value = "";
            document.getElementById('fpm4').value = "";
            document.getElementById('fpm5').value = "";
            document.getElementById('finalCFM').value = "";
            document.getElementById('avgReading').value = "";
        }

        function generateOutputQR(data) {
            const container = document.getElementById("qr-container");
            const qrDiv = document.getElementById("qrcode");
            
            container.style.display = "block";
            qrDiv.innerHTML = "";
            
            // This is the data hidden inside the Result QR Code
            const qrText = `SN:${data.serial}\nMotor:${data.motor}\nCFM:${data.cfm}\nStatus:${data.remark}`;
            
            new QRCode(qrDiv, {
                text: qrText,
                width: 160,
                height: 160
            });
            
            container.scrollIntoView({behavior: "smooth"});
        }

        // --- 4. EXPORT & TABLE ---
        function renderTable() {
            const logs = JSON.parse(localStorage.getItem('motorLogsV4')) || [];
            const tbody = document.querySelector('#logTable tbody');
            document.getElementById('count').innerText = logs.length;
            tbody.innerHTML = "";
            
            logs.slice(0, 10).forEach(log => {
                tbody.innerHTML += `<tr>
                    <td>${log.serial}</td>
                    <td>${log.motor}</td>
                    <td style="font-weight:bold; color:#0d47a1">${log.cfm}</td>
                    <td>${log.remark}</td>
                </tr>`;
            });
        }

        function downloadCSV() {
            const logs = JSON.parse(localStorage.getItem('motorLogsV4')) || [];
            if(logs.length === 0) return alert("No data!");

            let csv = "Date,Technician,Serial No,Motor Type,FPM Readings,Multiplier,Avg Reading,Final CFM,Pump Status,Auto Swing,Remark\n";
            logs.forEach(l => {
                csv += `${l.date},${l.tech},${l.serial},${l.motor},"${l.fpm}",${l.multi},${l.avg},${l.cfm},${l.pump},${l.swing},"${l.remark}"\n`;
            });
            
            const link = document.createElement("a");
            link.href = encodeURI("data:text/csv;charset=utf-8," + csv);
            link.download = "Motor_Log_" + new Date().toISOString().slice(0,10) + ".csv";
            document.body.appendChild(link);
            link.click();
        }

        function clearData() {
            if(confirm("Delete ALL data?")) {
                localStorage.removeItem('motorLogsV4');
                renderTable();
                document.getElementById("qr-container").style.display = "none";
            }
        }
    </script>
</body>
</html>
