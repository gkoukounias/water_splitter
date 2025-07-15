<!DOCTYPE html>
<html lang="el">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Κατανομή Λογαριασμού Νερού</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 2rem; background: #f0f0f0; }
    h1 { color: #0055aa; }
    input { margin: 0.3rem 0; padding: 0.4rem; width: 100%; box-sizing: border-box; }
    button { padding: 0.6rem 1rem; background: #007acc; color: white; border: none; margin-top: 1rem; cursor: pointer; }
    .result { background: white; padding: 1rem; border-radius: 8px; margin-top: 1rem; }
    label { font-weight: bold; margin-top: 0.6rem; display: block; }
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
      const rawValues = Array.from(inputs).map(input => parseFloat(input.value) || 0);
      const declaredTotal = rawValues.reduce((sum, val) => sum + val, 0);

      let adjustedValues;

      if (declaredTotal === 0) {
        // Avoid division by zero
        adjustedValues = rawValues.map(() => totalConsumption / rawValues.length);
      } else if (declaredTotal !== totalConsumption) {
        const diff = totalConsumption - declaredTotal;
        adjustedValues = rawValues.map(val => val + (val / declaredTotal) * diff);
      } else {
        adjustedValues = rawValues;
      }

      const resultHTML = adjustedValues.map((val, idx) => {
        const share = (val / totalConsumption) * totalBill;
        return `<p><strong>Ακίνητο ${idx + 1}:</strong> Κατανάλωση: ${val.toFixed(2)} m³ — Ποσό: €${share.toFixed(2)}</p>`;
      }).join('');

      document.getElementById('result').innerHTML = resultHTML;
    }
  </script>
</body>
</html>
