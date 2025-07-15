<!DOCTYPE html>
<html lang="el">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Κατανομή Λογαριασμού Νερού</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 2rem; background: #f4f4f4; }
    h1 { color: #0077cc; }
    input { margin: 0.5rem 0; padding: 0.5rem; width: 100%; }
    button { padding: 0.5rem 1rem; background: #0077cc; color: white; border: none; cursor: pointer; margin-top: 1rem; }
    .result { background: white; padding: 1rem; margin-top: 1rem; border-radius: 5px; }
  </style>
</head>
<body>
  <h1>Κατανομή Λογαριασμού Νερού</h1>

  <label>Συνολικό Ποσό (€):</label>
  <input type="number" id="totalBill" step="0.01">

  <label>Συνολική Κατανάλωση (m³):</label>
  <input type="number" id="totalConsumption" step="0.01">

  <label>Κατανάλωση Ακινήτου 1:</label>
  <input type="number" class="consumption" step="0.01">
  <label>Κατανάλωση Ακινήτου 2:</label>
  <input type="number" class="consumption" step="0.01">
  <label>Κατανάλωση Ακινήτου 3:</label>
  <input type="number" class="consumption" step="0.01">
  <label>Κατανάλωση Ακινήτου 4:</label>
  <input type="number" class="consumption" step="0.01">
  <label>Κατανάλωση Ακινήτου 5:</label>
  <input type="number" class="consumption" step="0.01">

  <button onclick="calculateShares()">Υπολογισμός</button>

  <div id="result" class="result"></div>

  <script>
    function calculateShares() {
      const totalBill = parseFloat(document.getElementById('totalBill').value);
      const totalConsumption = parseFloat(document.getElementById('totalConsumption').value);
      const inputs = document.querySelectorAll('.consumption');
      const values = Array.from(inputs).map(input => parseFloat(input.value) || 0);

      const declaredConsumption = values.reduce((sum, val) => sum + val, 0);
      const remainder = totalConsumption - declaredConsumption;
      const equalShare = remainder > 0 ? remainder / values.length : 0;
      const finalConsumptions = values.map(v => v + equalShare);

      const resultHTML = finalConsumptions.map((val, idx) => {
        const share = (val / totalConsumption) * totalBill;
        return `<p><strong>Ακίνητο ${idx + 1}:</strong> Κατανάλωση: ${val.toFixed(2)} m³ — Ποσό: €${share.toFixed(2)}</p>`;
      }).join('');

      document.getElementById('result').innerHTML = resultHTML;
    }
  </script>
</body>
</html>
