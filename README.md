#html
Kakulator lensa
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulator Lensa & Simulasi Mata Rabun</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f0f2f5; margin: 0; padding: 20px; color: #333; }
        .container { max-width: 600px; margin: auto; background: white; padding: 25px; border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
        hh2 { text-align: center; color: #007bff; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; }
        select, input { width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 6px; box-sizing: border-box; }
        button { width: 100%; padding: 12px; background: #007bff; color: white; border: none; border-radius: 6px; font-size: 16px; cursor: pointer; }
        button:hover { background: #0056b3; }
        .result-box { margin-top: 20px; padding: 15px; background: #e9ecef; border-radius: 6px; text-align: center; font-size: 18px; font-weight: bold; }
        .simulation-container { margin-top: 25px; text-align: center; }
        .simulation-screen { width: 100%; height: 180px; background: #222; border-radius: 8px; display: flex; align-items: center; justify-content: center; overflow: hidden; position: relative; }
        .object { color: white; font-size: 24px; font-weight: bold; transition: filter 0.5s ease; }
        .blur { filter: blur(8px); }
        .clear { filter: blur(0px); }
    </style>
</head>
<body>

<div class="container">
    <h2>Kalkulator Ukuran Lensa & Simulasi Mata</h2>
    
    <div class="form-group">
        <label for="defectType">Jenis Kelainan Mata:</label>
        <select id="defectType" onchange="toggleInputLabel()">
            <option value="myopia">Miopi (Rabun Jauh)</option>
            <option value="hypermetropia">Hipermetropi (Rabun Dekat)</option>
        </select>
    </div>

    <div class="form-group">
        <label id="labelDistance" for="distanceValue">Titik Jauh / Far Point (PR dalam cm):</label>
        <input type="number" id="distanceValue" placeholder="Contoh: 200" required>
    </div>

    <button onclick="calculateLens()">Hitung & Simulasi</button>

    <div class="result-box" id="resultText">
        Kekuatan Lensa: - Dioptri
    </div>

    <div class="simulation-container">
        <p>Simulasi Pandangan Objek ("EDUKASI FISIKA"):</p>
        <div class="simulation-screen">
            <div id="simObject" class="object blur">EDUKASI FISIKA</div>
        </div>
        <p id="statusSimulasi" style="font-size: 14px; color: #666; margin-top: 5px;">Status: Sebelum Koreksi (Kabur)</p>
        <button onclick="toggleCorrection()" style="background: #28a745; margin-top: 10px;">Terapkan Koreksi Lensa</button>
    </div>
</div>

<script>
    let isCorrected = false;

    function toggleInputLabel() {
        const type = document.getElementById('defectType').value;
        const label = document.getElementById('labelDistance');
        if (type === 'myopia') {
            label.innerText = "Titik Jauh / Far Point (PR dalam cm):";
        } else {
            label.innerText = "Titik Dekat / Near Point (PP dalam cm):";
        }
    }

    function calculateLens() {
        const type = document.getElementById('defectType').value;
        const val = parseFloat(document.getElementById('distanceValue').value);
        const resultBox = document.getElementById('resultText');

        if (!val || val <= 0) {
            alert("Masukkan angka yang valid!");
            return;
        }

        let power = 0;
        if (type === 'myopia') {
            // Rumus Miopi: P = -100 / PR (cm)
            power = -100 / val;
            resultBox.innerHTML = `Kekuatan Lensa Kacamata: <span style="color: red;">${power.toFixed(2)} Dioptri (Lensa Cekung / Negatif)</span>`;
        } else {
            // Rumus Hipermetropi: P = (100 / 25) - (100 / PP) = 4 - (100 / PP)
            power = 4 - (100 / val);
            resultBox.innerHTML = `Kekuatan Lensa Kacamata: <span style="color: green;">+${power.toFixed(2)} Dioptri (Lensa Cembung / Positif)</span>`;
        }
        
        // Reset simulasi ke kondisi awal
        isCorrected = false;
        document.getElementById('simObject').className = "object blur";
        document.getElementById('statusSimulasiinnerText').innerText = "Status: Sebelum Koreksi (Kabur)";
    }

    function toggleCorrection() {
        const simObj = document.getElementById('simObject');
        const statusText = document.getElementById('statusSimulasi');
        
        isCorrected = !isCorrected;
        if (isCorrected) {
            simObj.className = "object clear";
            statusText.innerText = "Status: Sesudah Koreksi (Jelas dengan Lensa)";
        } else {
            simObj.className = "object blur";
            statusText.innerText = "Status: Sebelum Koreksi (Kabur)";
        }
    }
</script>

</body>
</html>
