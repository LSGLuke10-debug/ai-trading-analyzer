# ai-trading-analyzer
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SmartScalp Dashboard</title>

<style>
:root {
  --bg: #0f172a;
  --card: #1e293b;
  --text: #ffffff;
  --accent: #38bdf8;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: var(--bg);
  color: var(--text);
}

header {
  background: #020617;
  padding: 15px;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}

.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 15px;
  padding: 15px;
}

.card {
  background: var(--card);
  padding: 15px;
  border-radius: 12px;
}

button {
  background: var(--accent);
  border: none;
  padding: 10px;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  width: 100%;
  margin-top: 8px;
}

input, select {
  width: 100%;
  padding: 8px;
  margin-top: 6px;
  border-radius: 6px;
  border: none;
}

img {
  max-width: 100%;
  margin-top: 10px;
  border-radius: 8px;
}
</style>
</head>

<body>

<header>SmartScalp Dashboard</header>

<div class="dashboard">

  <!-- Email Capture -->
  <div class="card">
    <h3>Join SmartScalp</h3>
    <input type="email" id="email" placeholder="Enter your email">
    <button onclick="saveEmail()">Submit</button>
    <p id="emailMsg"></p>
  </div>

  <!-- Screenshot Analyzer -->
  <div class="card">
    <h3>Chart Screenshot Analyzer</h3>
    <input type="file" accept="image/*" onchange="previewImage(event)">
    <img id="chartPreview" style="display:none;">
    <button onclick="analyzeTrend('bullish')">Bullish Trend</button>
    <button onclick="analyzeTrend('bearish')">Bearish Trend</button>
    <p id="imageResult"></p>
  </div>

  <!-- Risk Calculator -->
  <div class="card">
    <h3>Risk Calculator</h3>
    <input type="number" id="balance" placeholder="Account Balance">
    <input type="number" id="risk" placeholder="Risk %">
    <button onclick="calculateRisk()">Calculate</button>
    <p id="riskResult"></p>
  </div>

  <!-- AI Trade Analyzer -->
  <div class="card">
    <h3>AI Trade Analyzer</h3>
    <input type="number" id="entryPrice" placeholder="Entry Price" step="0.0001">
    <select id="style">
      <option value="scalp">Scalping</option>
      <option value="day">Day Trading</option>
    </select>
    <button onclick="generateSLTP()">Generate SL & TP</button>
    <p id="aiResult"></p>
  </div>

</div>

<script>
// 📧 Email Capture
function saveEmail() {
  const email = document.getElementById("email").value;
  if (!email) return;

  localStorage.setItem("userEmail", email);
  document.getElementById("emailMsg").innerText =
    "✅ Email saved! Welcome to SmartScalp.";
}

// 🖼 Screenshot Preview
function previewImage(event) {
  const img = document.getElementById("chartPreview");
  img.src = URL.createObjectURL(event.target.files[0]);
  img.style.display = "block";
}

// 📊 Analyze Trend from Screenshot
function analyzeTrend(trend) {
  const entry = parseFloat(document.getElementById("entryPrice").value);
  if (!entry) {
    document.getElementById("imageResult").innerText =
      "Enter entry price for analysis.";
    return;
  }

  let sl, tp;

  if (trend === "bullish") {
    sl = entry - 0.0010;
    tp = entry + 0.0020;
  } else {
    sl = entry + 0.0010;
    tp = entry - 0.0020;
  }

  document.getElementById("imageResult").innerHTML = `
    Trend Detected: <b>${trend.toUpperCase()}</b><br>
    Suggested SL: <b>${sl.toFixed(4)}</b><br>
    Suggested TP: <b>${tp.toFixed(4)}</b>
  `;
}

// 🧮 Risk Calculator
function calculateRisk() {
  const balance = document.getElementById("balance").value;
  const risk = document.getElementById("risk").value;
  if (!balance || !risk) return;

  const riskAmount = (balance * risk) / 100;
  document.getElementById("riskResult").innerText =
    "Risk Amount: $" + riskAmount.toFixed(2);
}

// 🤖 AI SL/TP Generator
function generateSLTP() {
  const entry = parseFloat(document.getElementById("entryPrice").value);
  const style = document.getElementById("style").value;

  if (!entry) return;

  const sl = style === "scalp" ? entry - 0.0005 : entry - 0.0015;
  const tp = style === "scalp" ? entry + 0.0008 : entry + 0.0030;

  document.getElementById("aiResult").innerHTML = `
    Stop Loss: <b>${sl.toFixed(4)}</b><br>
    Take Profit: <b>${tp.toFixed(4)}</b><br>
    Strategy: ${style}
  `;
}
</script>

</body>
</html>
