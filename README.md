<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Aplikacja Walutowa</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <div id="home" class="screen active">
      <div class="top">Aplikacja Walutowa - Strona główna</div>
      <div class="grid2">
        <button onclick="showScreen('settings')">Ustawienia konta</button>
        <button onclick="showScreen('app')">Ustawienia aplikacji</button>
      </div>
      <div class="grid3">
        <button onclick="showScreen('rates')">Aktualne kursy</button>
        <button onclick="showScreen('history')">Historia kursów</button>
        <button onclick="showScreen('calculator')">Kalkulator</button>
      </div>
    </div>

    <div id="calculator" class="screen">
      <button class="back" onclick="showScreen('home')">Powrót</button>
      <h2>Kalkulator walut</h2>
      <input id="amount" type="text" inputmode="decimal" placeholder="Kwota">
      <select id="from"><option>PLN</option><option>USD</option><option>EUR</option><option>GBP</option><option>CHF</option></select>
      <select id="to"><option>USD</option><option>PLN</option><option>EUR</option><option>GBP</option><option>CHF</option></select>
      <button onclick="convertCurrency()">Przelicz</button>
      <input id="result" readonly placeholder="Wynik">
    </div>

    <div id="rates" class="screen">
      <button class="back" onclick="showScreen('home')">Powrót</button>
      <h2>Aktualne kursy</h2>
      <div class="line">USD = 3.85 PLN</div>
      <div class="line">EUR = 4.20 PLN</div>
      <div class="line">GBP = 4.80 PLN</div>
      <div class="line">CHF = 4.35 PLN</div>
      <div class="line">PLN = 1.00 PLN</div>
    </div>

    <div id="history" class="screen">
      <button class="back" onclick="showScreen('home')">Powrót</button>
      <h2>Historia kursów</h2>
      <div id="historyList"></div>
    </div>

    <div id="settings" class="screen">
      <button class="back" onclick="showScreen('home')">Powrót</button>
      <h2>Ustawienia konta</h2>
      <input placeholder="Imię i nazwisko">
      <input placeholder="Email">
      <input type="password" placeholder="Hasło">
    </div>

    <div id="app" class="screen">
      <button class="back" onclick="showScreen('home')">Powrót</button>
      <h2>Ustawienia aplikacji</h2>
      <input placeholder="Domyślna waluta">
      <input placeholder="Język">
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
const rates = { PLN:1, USD:3.85, EUR:4.20, GBP:4.80, CHF:4.35 };
const historyData = [];

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(screen => {
    screen.classList.remove('active');
  });
  document.getElementById(id).classList.add('active');
}

function convertCurrency() {
  const amount = parseFloat(document.getElementById('amount').value) || 0;
  const from = document.getElementById('from').value;
  const to = document.getElementById('to').value;

  const inPln = amount * rates[from];
  const result = (inPln / rates[to]).toFixed(2);
  document.getElementById('result').value = result + ' ' + to;

  historyData.unshift(`${amount} ${from} = ${result} ${to}`);
  const list = document.getElementById('historyList');
  list.innerHTML = '';
  historyData.slice(0, 5).forEach(item => {
    list.innerHTML += `<div class="line">${item}</div>`;
  });
}
body { font-family: Arial, sans-serif; background:#ddd; }
.container { width: 420px; margin: 20px auto; }
.screen { display:none; background:white; padding:20px; box-shadow:0 0 8px rgba(0,0,0,.2); }
.active { display:block; }
.top { background:#67d84b; padding:18px; text-align:center; font-weight:bold; margin-bottom:10px; }
.grid2, .grid3 { display:grid; gap:10px; }
.grid2 { grid-template-columns:1fr 1fr; }
.grid3 { grid-template-columns:1fr 1fr 1fr; margin-top:10px; }
button { padding:10px; cursor:pointer; }
.back { margin-bottom:10px; }
input, select { display:block; width:100%; margin:10px 0; padding:10px; }
.line { border:1px solid #999; padding:10px; margin:8px 0; }
