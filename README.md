<!DOCTYPE html>
<html lang="el">
<head>
  <meta charset="UTF-8">
  <title>Κατανομή Λογαριασμού Νερού</title>
  <style>body { font-family: sans-serif; max-width: 600px; margin: auto; } input { width: 100%; padding: 0.5em; margin: 0.2em 0; }</style>
</head>
<body>
  <h2>Κατανομή Λογαριασμού Νερού</h2>
  <label>Συνολικός Λογαριασμός (€):<input id="totalCost" type="number" /></label>
  <label>Συνολική Κατανάλωση (m³):<input id="totalVol" type="number" /></label>
  <div id="meters"></div>
  <button onclick="calc()">Υπολόγισε</button>
  <h3>Αποτελέσματα:</h3>
  <div id="results"></div>
  <script>
    const tiers = [
      {limit:20,price:0.57}, {limit:40,price:0.90}, {limit:60,price:1.42}, {limit:Infinity,price:2.06}
    ];
    function calculate(volume) {
      let rem = volume, prev=0, cost=0;
      for(const t of tiers){
        let take = Math.min(t.limit - prev, rem);
        if(take<=0) break;
        cost += take * t.price;
        rem -= take; prev = t.limit;
      }
      return cost;
    }
    window.onload = () => {
      let html = '';
      for(let i=1;i<=5;i++){
        html += `<label>Κατανάλωση Ακινήτου ${i} (m³):<input id="c${i}" type="number" /></label>`;
      }
      document.getElementById('meters').innerHTML = html;
    };
    function calc(){
      const totalCost= parseFloat(document.getElementById('totalCost').value) ||0;
      const totalVol = parseFloat(document.getElementById('totalVol').value) ||0;
      const c = [];
      for(let i=1;i<=5;i++){
        c.push(parseFloat(document.getElementById('c'+i).value)||0);
      }
      const costs = c.map(v=>calculate(v));
      const sumCalc = costs.reduce((a,b)=>a+b,0);
      const diff = totalCost - sumCalc;
      const share = diff/5;
      let out = '<table border="1" cellpadding="5"><tr><th>Ακίνητο</th><th>Κόστος (€)</th></tr>';
      costs.forEach((x,i)=>{
        out += `<tr><td>${i+1}</td><td>${(x+share).toFixed(2)}</td></tr>`;
      });
      out += '</table>';
      document.getElementById('results').innerHTML = out;
    }
  </script>
</body>
</html>
