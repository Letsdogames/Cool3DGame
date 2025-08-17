
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kids' Stock Market Game</title>
  <style>
    /* Reset default margins and ensure box-sizing */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: Arial, sans-serif;
      background-color: #2c2f33;
      color: #e0e0e0;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 15px;
    }
    h1 {
      color: #4da8ff;
      font-size: clamp(1.5rem, 5vw, 2rem);
      font-weight: bold;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.4);
      margin: 15px 0 10px;
      text-align: center;
    }
    h2 {
      font-size: clamp(0.85rem, 3.5vw, 1rem);
      color: #d0d0d0;
      text-align: center;
      max-width: 90%;
      margin: 0 0 20px;
    }
    .container {
      display: flex;
      flex-direction: column;
      gap: 15px;
      width: 100%;
      max-width: 1200px;
      align-items: center;
      position: relative;
    }
    .panel {
      background-color: #3a3d41;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.3);
      width: 100%;
      max-width: 400px;
      position: relative;
    }
    #player-info, #portfolio, #update-note-container, #stock-menu, #predictions {
      transition: transform 0.2s ease;
    }
    #player-info:hover, #portfolio:hover, #stock-menu:hover, #predictions:hover {
      transform: translateY(-2px);
    }
    #stock-menu {
      max-height: 300px;
      overflow-y: auto;
      scroll-behavior: smooth;
    }
    #action-panel {
      display: none;
      background-color: #3a3d41;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.3);
      width: 100%;
      max-width: 400px;
    }
    #portfolio-list, #stocks-list, #predictions-list {
      position: relative;
    }
    .portfolio-item, .stock-item, .prediction-item {
      padding: 10px;
      border-bottom: 1px solid #4a4d51;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }
    .portfolio-item:hover, .stock-item:hover {
      background-color: #45484d;
    }
    .arrow-up {
      color: #28a745;
    }
    .arrow-down {
      color: #dc3545;
    }
    .bought-price {
      font-size: clamp(0.7rem, 2vw, 0.8rem);
      color: #b0b0b0;
      margin-left: 8px;
      display: inline;
    }
    .profit-loss {
      font-size: clamp(0.65rem, 1.8vw, 0.75rem);
      margin-top: 4px;
      display: block;
      padding-left: 18px;
    }
    .total-value {
      font-size: clamp(0.65rem, 1.8vw, 0.75rem);
      margin-top: 4px;
      display: block;
      padding-left: 18px;
      color: #b0b0b0;
    }
    #action-panel h3 {
      margin: 0 0 10px;
      font-size: clamp(1rem, 3vw, 1.2rem);
      padding-right: 40px;
      color: #e0e0e0;
    }
    #action-panel button {
      margin: 8px 4px;
      padding: 12px 20px;
      font-size: clamp(0.85rem, 2.5vw, 0.95rem);
      min-width: 100px;
      border-radius: 5px;
      border: none;
      background-color: #0066cc;
      color: white;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }
    #action-panel button:hover:not(:disabled) {
      background-color: #0052a3;
    }
    #action-panel button:disabled {
      background-color: #666;
      cursor: not-allowed;
    }
    #confirmation {
      margin-top: 15px;
      display: none;
    }
    #confirmation p {
      font-weight: bold;
      color: #ff6b6b;
      font-size: clamp(0.85rem, 2.5vw, 0.95rem);
    }
    #quantity-controls {
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 15px 0;
      gap: 8px;
    }
    #quantity {
      width: 70px;
      height: 36px;
      text-align: center;
      padding: 0 8px;
      font-size: clamp(0.85rem, 2.5vw, 0.95rem);
      border-radius: 5px;
      border: 1px solid #555;
      line-height: 36px;
      background-color: #45484d;
      color: #e0e0e0;
    }
    #minus-btn, #plus-btn {
      width: 36px;
      height: 36px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: clamp(0.85rem, 2.5vw, 0.95rem);
      border-radius: 5px;
      border: 1px solid #555;
      background-color: #0066cc;
      color: white;
      cursor: pointer;
      transition: background-color 0.2s ease;
    }
    #minus-btn:hover, #plus-btn:hover {
      background-color: #0052a3;
    }
    .note {
      font-size: clamp(0.7rem, 2vw, 0.8rem);
      color: #b0b0b0;
      margin-top: 12px;
      text-align: center;
    }
    .update-note {
      font-size: clamp(0.7rem, 2vw, 0.8rem);
      color: #d0d0d0;
      margin: 0;
    }
    #close-trade {
      position: absolute;
      top: 12px;
      right: 12px;
      font-size: 1.5rem;
      cursor: pointer;
      color: #e0e0e0;
      background-color: #3a3d41;
      padding: 6px;
      border-radius: 50%;
      line-height: 1;
      z-index: 100;
      transition: color 0.2s ease, background-color 0.2s ease;
    }
    #close-trade:hover {
      color: #4da8ff;
      background-color: #45484d;
    }
    @media (min-width: 768px) {
      .container {
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: flex-start;
        gap: 20px;
      }
      .panel {
        width: calc(33.33% - 14px);
        max-width: 380px;
      }
      #player-info, #portfolio, #update-note-container {
        width: calc(25% - 15px);
        max-width: 300px;
      }
      #stock-menu {
        max-height: 450px;
      }
      #action-panel {
        position: absolute;
        left: 420px;
        top: 200px;
        width: 380px;
      }
    }
    @media (max-width: 767px) {
      body {
        padding: 10px;
      }
      .panel {
        margin-bottom: 15px;
      }
      #stock-menu {
        max-height: 250px;
      }
      #action-panel {
        position: static;
        margin-bottom: 15px;
      }
      #action-panel button {
        width: calc(50% - 8px);
      }
      #quantity-controls {
        gap: 6px;
      }
      #quantity {
        margin: 0;
        height: 32px;
        line-height: 32px;
      }
      #minus-btn, #plus-btn {
        width: 32px;
        height: 32px;
        font-size: clamp(0.8rem, 2vw, 0.9rem);
      }
      #close-trade {
        font-size: 1.3rem;
        padding: 8px;
      }
    }
    @media (max-width: 480px) {
      h1 {
        font-size: clamp(1.3rem, 4.5vw, 1.7rem);
      }
      h2 {
        font-size: clamp(0.75rem, 3vw, 0.9rem);
      }
      #action-panel button {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <h1>Kids' Stock Market Game</h1>
  <h2>This is an example of how stocks work! Trade with fake money to learn, but beware: stocks are risky, and you could lose it all!</h2>
  <div class="container">
    <div id="player-info" class="panel">
      <h3>My Bank Account: $<span id="cash">400.00</span></h3>
    </div>
    <div id="portfolio" class="panel">
      <h3>My Portfolio</h3>
      <div id="portfolio-list"></div>
      <p class="note">Click on your stock to sell.</p>
    </div>
    <div id="update-note-container" class="panel">
      <p class="update-note">More updates coming soon!</p>
    </div>
    <div id="stock-menu" class="panel">
      <h3>Available Stocks</h3>
      <div id="stocks-list"></div>
    </div>
    <div id="predictions" class="panel">
      <h3>Predictions</h3>
      <div id="predictions-list"></div>
      <p class="note">The prediction is not 100% accurate, this is a value we got doing very complex math</p>
    </div>
    <div id="action-panel" class="panel">
      <span id="close-trade" aria-label="Close trade panel">✖</span>
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
      portfolio[stock.symbol] = { shares: 0, purchases: [] };
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
        predictedChanges[stock.symbol] = { current: parseFloat(currentPercent), max: parseFloat(max) };
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
        const stockData = portfolio[stock.symbol];
        if (stockData.shares > 0) {
          // Calculate average purchase price
          const totalCost = stockData.purchases.reduce((sum, purchase) => sum + purchase.price * purchase.quantity, 0);
          const totalShares = stockData.purchases.reduce((sum, purchase) => sum + purchase.quantity, 0);
          const avgPurchasePrice = totalCost / totalShares;
          // Calculate profit/loss percentage
          const profitLossPercent = ((stock.price - avgPurchasePrice) / avgPurchasePrice * 100).toFixed(1);
          const isProfit = stock.price >= avgPurchasePrice;
          const profitLossClass = isProfit ? 'arrow-up' : 'arrow-down';
          const profitLossText = isProfit ? `↑ ${profitLossPercent}% profit` : `↓ ${Math.abs(profitLossPercent)}% loss`;
          // Calculate total current value
          const totalValue = (stock.price * stockData.shares).toFixed(2);

          const item = document.createElement('div');
          item.classList.add('portfolio-item');
          item.innerHTML = `
            ${stock.name} (${stock.symbol}): ${stockData.shares} shares
            <span class="bought-price">↳ Bought at $${avgPurchasePrice.toFixed(2)}</span>
            <div class="profit-loss ${profitLossClass}">${profitLossText}</div>
            <div class="total-value">Total Value: $${totalValue}</div>
          `;
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
          const { current, max } = predictedChanges[stock.symbol];
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
        document.getElementById('sell-btn').disabled = portfolio[selectedStock.symbol].shares <= 0;
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
        if (actionType === 'sell' && portfolio[selectedStock.symbol].shares > 0) {
          quantity = Math.min(quantity, portfolio[selectedStock.symbol].shares);
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
      document.getElementById('sell-btn').disabled = portfolio[stock.symbol].shares <= 0;
      // Scroll to action-panel on mobile (≤ 767px)
      if (window.innerWidth <= 767) {
        const actionPanel = document.getElementById('action-panel');
        actionPanel.scrollIntoView({ behavior: 'smooth', block: 'start' });
      }
    }

    function showConfirmation(type) {
      actionType = type;
      const quantity = parseInt(document.getElementById('quantity').value) || 1;
      if (type === 'buy' && selectedStock.price * quantity > cash) {
        alert('Not enough cash to buy this amount!');
        return;
      }
      if (type === 'sell' && quantity > portfolio[selectedStock.symbol].shares) {
        alert(`You only own ${portfolio[selectedStock.symbol].shares} share(s) of ${selectedStock.name}!`);
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
          portfolio[selectedStock.symbol].shares += quantity;
          portfolio[selectedStock.symbol].purchases.push({ price: selectedStock.price, quantity });
          updateCash();
          updatePortfolio();
          alert('Purchase successful!');
        } else {
          alert('Not enough cash!');
        }
      } else if (actionType === 'sell') {
        if (portfolio[selectedStock.symbol].shares >= quantity) {
          cash += selectedStock.price * quantity;
          portfolio[selectedStock.symbol].shares -= quantity;
          // Remove oldest purchases first (FIFO)
          let sharesToRemove = quantity;
          while (sharesToRemove > 0 && portfolio[selectedStock.symbol].purchases.length > 0) {
            const purchase = portfolio[selectedStock.symbol].purchases[0];
            if (purchase.quantity <= sharesToRemove) {
              sharesToRemove -= purchase.quantity;
              portfolio[selectedStock.symbol].purchases.shift();
            } else {
              purchase.quantity -= sharesToRemove;
              sharesToRemove = 0;
            }
          }
          updateCash();
          updatePortfolio();
          alert('Sale successful!');
          document.getElementById('sell-btn').disabled = portfolio[selectedStock.symbol].shares <= 0;
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
      if (actionType === 'sell' && portfolio[selectedStock.symbol].shares > 0) {
        quantity = Math.min(quantity + 1, portfolio[selectedStock.symbol].shares);
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
      if (actionType === 'sell' && portfolio[selectedStock.symbol].shares > 0) {
        quantity = Math.min(quantity, portfolio[selectedStock.symbol].shares);
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
