
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kids' Stock Market Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #808080;
      color: #333;
      position: relative;
    }
    h1 {
      color: #0066cc;
      font-size: 2.5em;
      font-weight: bold;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
      margin: 20px 0 10px 0;
    }
    h2 {
      font-size: 1.2em;
      color: #FFFFFF;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
      text-align: center;
      max-width: 600px;
      margin: 0 0 10px 0;
    }
    #player-info {
      position: absolute;
      right: 20px;
      top: 140px;
      background-color: #fff;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    #portfolio {
      position: absolute;
      right: 20px;
      top: 220px;
      width: 200px;
      background-color: #fff;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    #portfolio-list {
      position: relative;
    }
    .portfolio-item {
      padding: 5px;
      border-bottom: 1px solid #ddd;
      cursor: pointer;
    }
    #update-note-container {
      position: absolute;
      right: 20px;
      top: 370px;
      width: 200px;
      text-align: center;
    }
    #stock-menu {
      width: 300px;
      height: 240px;
      overflow-y: auto;
      scroll-behavior: smooth;
      background-color: #fff;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      position: absolute;
      left: 20px;
      top: 200px;
    }
    #stocks-list {
      position: relative;
    }
    .stock-item {
      padding: 5px;
      border-bottom: 1px solid #ddd;
      cursor: pointer;
    }
    .arrow-up {
      color: green;
    }
    .arrow-down {
      color: red;
    }
    #action-panel {
      width: 300px;
      background-color: #fff;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      position: absolute;
      left: 340px;
      top: 200px;
      display: none;
    }
    #action-panel h3 {
      margin-top: 0;
    }
    #action-panel button {
      margin: 5px;
      padding: 10px;
      font-size: 1em;
      cursor: pointer;
    }
    #confirmation {
      margin-top: 10px;
      display: none;
    }
    #confirmation p {
      font-weight: bold;
      color: #ff0000;
    }
    #quantity-controls {
      display: flex;
      align-items: center;
      margin: 10px 0;
    }
    #quantity {
      width: 50px;
      text-align: center;
      margin: 0 10px;
    }
    #predictions {
      width: 300px;
      background-color: #fff;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      position: absolute;
      left: 20px;
      top: 470px;
    }
    #predictions-list {
      position: relative;
    }
    .prediction-item {
      padding: 5px;
      border-bottom: 1px solid #ddd;
    }
    .note {
      font-size: 0.8em;
      color: #666;
      margin-top: 10px;
    }
    #close-trade {
      position: absolute;
      top: 5px;
      right: 10px;
      font-size: 1.2em;
      cursor: pointer;
      color: #333;
    }
    .update-note {
      font-size: 0.8em;
      color: #fff;
      margin-top: 0;
    }
  </style>
</head>
<body>
  <h1>Kids' Stock Market Game</h1>
  <h2>This is an example of how stocks work! Trade with fake money to learn, but beware: stocks are risky, and you could lose it all!</h2>
  <div id="player-info">
    <h3>My Bank Account: $<span id="cash">400.00</span></h3>
  </div>
  <div id="portfolio">
    <h3>My Portfolio</h3>
    <div id="portfolio-list"></div>
    <p class="note">Click on your stock to sell.</p>
  </div>
  <div id="update-note-container">
    <p class="update-note">More updates coming soon!</p>
  </div>
  <div id="stock-menu">
    <h3>Available Stocks</h3>
    <div id="stocks-list"></div>
  </div>
  <div id="predictions">
    <h3>Predictions</h3>
    <div id="predictions-list"></div>
    <p class="note">The prediction is not 100% accurate, this is a value we got doing very complex math</p>
  </div>
  <div id="action-panel">
    <span id="close-trade">✖</span>
    <h3>Trade <span id="selected-stock-name"></span></h3>
    <p>Total Cost: $<span id="selected-stock-price"></span></p>
    <button id="buy-btn">Buy</button>
    <button id="sell-btn">Sell</button>
    <div id="quantity-controls">
      <button id="minus-btn">-</button>
      <input id="quantity" type="number" value="1" min="1">
      <button id="plus-btn">+</button>
    </div>
    <div id="confirmation">
      <p id="confirm-message"></p>
      <button id="confirm-yes">Yes</button>
      <button id="confirm-no">No</button>
    </div>
  </div>
  <script>
    const stocks = [
      { symbol: 'FUN', name: 'FunToys Inc.', price: 100 },
      { symbol: 'CANDY', name: 'CandyWorld', price: 50 },
      { symbol: 'HERO', name: 'SuperHeroes Ltd.', price: 200 },
      { symbol: 'PLAY', name: 'PlayLand Co.', price: 75 },
      { symbol: 'MAGIC', name: 'MagicAdventures', price: 150 },
      { symbol: 'DREAM', name: 'DreamWorld Ent.', price: 120 },
      { symbol: 'ADVENT', name: 'AdventurePark', price: 90 },
      { symbol: 'ZOOM', name: 'ZoomRides Co.', price: 80 },
      { symbol: 'SPARK', name: 'SparkleGames', price: 110 },
      { symbol: 'JUMP', name: 'JumpStar Toys', price: 60 },
      { symbol: 'GLOW', name: 'GlowFun Inc.', price: 130 },
      { symbol: 'BUDDY', name: 'PetBuddy Co.', price: 95 },
      { symbol: 'WIZ', name: 'WizardWorld', price: 140 },
      { symbol: 'CUBE', name: 'CubeCraft Ltd.', price: 85 },
      { symbol: 'BLAST', name: 'BlastOff Ent.', price: 115 },
      { symbol: 'RACE', name: 'SpeedyRaces', price: 70 },
      { symbol: 'STAR', name: 'StarShine Co.', price: 125 },
      { symbol: 'TRICK', name: 'TrickTreats', price: 65 },
      { symbol: 'FLY', name: 'SkyFly Adventures', price: 135 },
      { symbol: 'NEO', name: 'NeoPlay Inc.', price: 100 },
      { symbol: 'ZAP', name: 'ZapZone Ent.', price: 90 },
      { symbol: 'KIDZ', name: 'KidzKool Co.', price: 80 },
      { symbol: 'SPIN', name: 'SpinFun Toys', price: 110 },
      { symbol: 'ROAR', name: 'DinoRoar Ltd.', price: 120 },
      { symbol: 'SHINE', name: 'BrightShine', price: 75 },
      { symbol: 'BOOM', name: 'BoomPlay Co.', price: 130 },
      { symbol: 'WAVE', name: 'WaveRiders', price: 85 },
      { symbol: 'ZEST', name: 'ZestyGames', price: 95 },
      { symbol: 'POP', name: 'PopFizz Inc.', price: 105 },
      { symbol: 'RUSH', name: 'RushRides Ent.', price: 115 },
      { symbol: 'GLIM', name: 'GlimmerToys', price: 70 },
      { symbol: 'VROOM', name: 'VroomCars Co.', price: 125 },
      { symbol: 'SKY', name: 'SkyHigh Ltd.', price: 80 },
      { symbol: 'BOP', name: 'BopNPlay', price: 90 },
      { symbol: 'ZIP', name: 'ZippyFun Co.', price: 100 },
      { symbol: 'TWIRL', name: 'TwirlGames', price: 110 },
      { symbol: 'BOLT', name: 'BoltAdventures', price: 120 },
      { symbol: 'FLASH', name: 'FlashFun Inc.', price: 95 },
      { symbol: 'GIG', name: 'GiggleWorld', price: 85 },
      { symbol: 'SPREE', name: 'FunSpree Ent.', price: 105 },
      { symbol: 'WHIZ', name: 'WhizKid Toys', price: 115 }
    ];

    let cash = 400;
    const portfolio = {};
    stocks.forEach(stock => {
      portfolio[stock.symbol] = 0;
      const changePercent = (Math.random() - 0.5) * 10;
      stock.prevPrice = stock.price / (1 + changePercent / 100);
      stock.price = Math.max(10, Math.min(500, stock.price));
    });

    let selectedStock = null;
    let actionType = null;
    let predictedChanges = {};

    function generatePredictions() {
      predictedChanges = {};
      const selectedStocks = stocks.sort(() => Math.random() - 0.5).slice(0, 6);
      const list = document.getElementById('predictions-list');
      list.innerHTML = '';
      selectedStocks.forEach(stock => {
        const currentPercent = stock.prevPrice ? ((stock.price - stock.prevPrice) / stock.prevPrice * 100).toFixed(1) : 0;
        const max = (parseFloat(currentPercent) + Math.random() * (20 - currentPercent)).toFixed(1);
        predictedChanges[stock.symbol] = {current: parseFloat(currentPercent), max: parseFloat(max)};
        const item = document.createElement('div');
        item.classList.add('prediction-item');
        item.innerHTML = `${stock.name} may go up ${currentPercent}% to ${max}% in a minute.`;
        list.appendChild(item);
      });
    }

    function updatePortfolio() {
      const list = document.getElementById('portfolio-list');
      list.innerHTML = '';
      stocks.forEach(stock => {
        if (portfolio[stock.symbol] > 0) {
          const item = document.createElement('div');
          item.classList.add('portfolio-item');
          item.innerHTML = `${stock.name} (${stock.symbol}): ${portfolio[stock.symbol]} shares`;
          item.onclick = () => selectStock(stock);
          list.appendChild(item);
        }
      });
    }

    function updateStocks() {
      stocks.forEach(stock => {
        stock.prevPrice = stock.price;
        let changePercent;
        if (predictedChanges[stock.symbol] && Math.random() < 0.6) {
          const {current, max} = predictedChanges[stock.symbol];
          const variation = (Math.random() * 4 - 2);
          changePercent = Math.min(max, Math.max(current, current + variation));
        } else {
          changePercent = (Math.random() - 0.5) * 10;
        }
        stock.price = Math.max(10, Math.min(500, stock.price * (1 + changePercent / 100)));
      });
      displayStocks();
      updatePortfolio();
      if (selectedStock) {
        updateTotalCost();
        document.getElementById('sell-btn').disabled = portfolio[selectedStock.symbol] <= 0;
      }
    }

    function displayStocks() {
      const list = document.getElementById('stocks-list');
      list.innerHTML = '';
      stocks.forEach(stock => {
        const changePercent = stock.prevPrice ? ((stock.price - stock.prevPrice) / stock.prevPrice * 100).toFixed(1) : 0;
        let arrowClass, arrowText;
        if (stock.price > stock.prevPrice) {
          arrowClass = 'arrow-up';
          arrowText = `↑ ${changePercent}%`;
        } else {
          arrowClass = 'arrow-down';
          arrowText = `↓ ${Math.abs(changePercent)}%`;
        }
        const item = document.createElement('div');
        item.classList.add('stock-item');
        item.innerHTML = `${stock.name} (${stock.symbol}): $${stock.price.toFixed(2)} <span class="${arrowClass}">${arrowText}</span>`;
        item.onclick = () => selectStock(stock);
        list.appendChild(item);
      });
    }

    function updateTotalCost() {
      if (selectedStock) {
        const quantityInput = document.getElementById('quantity');
        let quantity = parseInt(quantityInput.value) || 1;
        if (actionType === 'sell' && portfolio[selectedStock.symbol] > 0) {
          quantity = Math.min(quantity, portfolio[selectedStock.symbol]);
          quantityInput.value = quantity;
        }
        const totalCost = (selectedStock.price * quantity).toFixed(2);
        document.getElementById('selected-stock-price').textContent = totalCost;
        if (document.getElementById('confirmation').style.display === 'block') {
          const shareText = quantity === 1 ? 'share' : 'shares';
          const message = `Are you sure you want to ${actionType} ${quantity} ${shareText} of ${selectedStock.name}? Do it at your own risk!`;
          document.getElementById('confirm-message').textContent = message;
        }
      }
    }

    function updateCash() {
      document.getElementById('cash').textContent = cash.toFixed(2);
    }

    function selectStock(stock) {
      selectedStock = stock;
      document.getElementById('selected-stock-name').textContent = stock.name;
      document.getElementById('quantity').value = 1;
      updateTotalCost();
      document.getElementById('action-panel').style.display = 'block';
      document.getElementById('confirmation').style.display = 'none';
      document.getElementById('sell-btn').disabled = portfolio[stock.symbol] <= 0;
    }

    function showConfirmation(type) {
      actionType = type;
      const quantity = parseInt(document.getElementById('quantity').value) || 1;
      if (type === 'buy' && selectedStock.price * quantity > cash) {
        alert('Not enough cash to buy this amount!');
        return;
      }
      if (type === 'sell' && quantity > portfolio[selectedStock.symbol]) {
        alert(`You only own ${portfolio[selectedStock.symbol]} share(s) of ${selectedStock.name}!`);
        return;
      }
      const shareText = quantity === 1 ? 'share' : 'shares';
      const message = `Are you sure you want to ${type} ${quantity} ${shareText} of ${selectedStock.name}? Do it at your own risk!`;
      document.getElementById('confirm-message').textContent = message;
      document.getElementById('action-panel').style.display = 'block';
      document.getElementById('confirmation').style.display = 'block';
    }

    function confirmAction(yes) {
      document.getElementById('confirmation').style.display = 'none';
      if (!yes) return;

      const quantity = parseInt(document.getElementById('quantity').value) || 1;
      if (actionType === 'buy') {
        const totalCost = selectedStock.price * quantity;
        if (cash >= totalCost) {
          cash -= totalCost;
          portfolio[selectedStock.symbol] += quantity;
          updateCash();
          updatePortfolio();
          alert('Purchase successful!');
        } else {
          alert('Not enough cash!');
        }
      } else if (actionType === 'sell') {
        if (portfolio[selectedStock.symbol] >= quantity) {
          cash += selectedStock.price * quantity;
          portfolio[selectedStock.symbol] -= quantity;
          updateCash();
          updatePortfolio();
          alert('Sale successful!');
          document.getElementById('sell-btn').disabled = portfolio[selectedStock.symbol] <= 0;
        } else {
          alert('Not enough shares to sell!');
        }
      }
      updateTotalCost();
    }

    document.getElementById('buy-btn').onclick = () => showConfirmation('buy');
    document.getElementById('sell-btn').onclick = () => showConfirmation('sell');
    document.getElementById('confirm-yes').onclick = () => confirmAction(true);
    document.getElementById('confirm-no').onclick = () => confirmAction(false);
    document.getElementById('plus-btn').onclick = () => {
      const quantityInput = document.getElementById('quantity');
      let quantity = parseInt(quantityInput.value) || 1;
      if (actionType === 'sell' && portfolio[selectedStock.symbol] > 0) {
        quantity = Math.min(quantity + 1, portfolio[selectedStock.symbol]);
      } else {
        quantity += 1;
      }
      quantityInput.value = quantity;
      updateTotalCost();
    };
    document.getElementById('minus-btn').onclick = () => {
      const quantityInput = document.getElementById('quantity');
      const newValue = Math.max(1, parseInt(quantityInput.value) - 1);
      quantityInput.value = newValue;
      updateTotalCost();
    };
    document.getElementById('quantity').oninput = () => {
      const quantityInput = document.getElementById('quantity');
      let quantity = parseInt(quantityInput.value) || 1;
      if (quantity < 1) {
        quantity = 1;
      }
      if (actionType === 'sell' && portfolio[selectedStock.symbol] > 0) {
        quantity = Math.min(quantity, portfolio[selectedStock.symbol]);
      }
      quantityInput.value = quantity;
      updateTotalCost();
    };
    document.getElementById('close-trade').onclick = () => {
      document.getElementById('action-panel').style.display = 'none';
      selectedStock = null;
      actionType = null;
    };

    generatePredictions();
    displayStocks();
    updateCash();
    updatePortfolio();
    setInterval(updateStocks, 30000);
    setInterval(generatePredictions, 60000);
  </script>
</body>
</html>
