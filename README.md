# Star-Collector-Game<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Star Collector</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #1a1a2e;
            color: #e6e6e6;
            margin: 0;
            user-select: none;
        }
        #game-container {
            display: flex;
            gap: 20px;
            padding: 20px;
            background-color: #16213e;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            margin-top: 20px;
        }
        #main-area {
            text-align: center;
            min-width: 200px;
        }
        #star {
            width: 120px;
            height: 120px;
            cursor: pointer;
            transition: transform 0.1s;
            filter: drop-shadow(0 0 5px #ffff99);
        }
        #star:active {
            transform: scale(0.9);
            filter: drop-shadow(0 0 10px #ffff99);
        }
        #star-count, #stars-per-second {
            font-size: 20px;
            margin: 10px 0;
        }
        #star-types, #upgrades {
            display: flex;
            flex-direction: column;
            gap: 8px;
            min-width: 220px;
            max-height: 400px;
            overflow-y: auto;
        }
        button {
            padding: 8px 16px;
            font-size: 14px;
            background-color: #0f3460;
            color: #e6e6e6;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        button:hover {
            background-color: #1f4a8a;
        }
        button:disabled {
            background-color: #4a4a4a;
            cursor: not-allowed;
        }
        #save-load {
            margin-top: 20px;
            display: flex;
            gap: 10px;
        }
        .star-type {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px;
            border-radius: 5px;
            font-size: 14px;
        }
        .star-type.blue { background-color: #00b7eb; color: #000; }
        .star-type.red { background-color: #ff4040; color: #fff; }
        .star-type.gold { background-color: #ffd700; color: #000; }
        .star-type.cosmic { background-color: #800080; color: #fff; }
        .star-type.nebula { background-color: #ff69b4; color: #000; }
        .star-type.pulsar { background-color: #00ff7f; color: #000; }
        .star-type.supernova { background-color: #ff4500; color: #fff; }
        .star-type.black-hole { background-color: #2f2f2f; color: #fff; border: 1px solid #fff; }
    </style>
</head>
<body>
    <div id="game-container">
        <div id="main-area">
            <h1>Star Collector</h1>
            <img id="star" src="https://img.icons8.com/emoji/120/000000/star-emoji.png" alt="Star">
            <p id="star-count">Stars: 0</p>
            <p id="stars-per-second">Stars per second: 0</p>
        </div>
        <div id="star-types">
            <h3>Star Types</h3>
            <div id="blue-star" class="star-type blue">
                <span>Blue Star: 0 (0.1/s)</span>
                <button onclick="buyStarType('blue')">Buy (15 Stars)</button>
            </div>
            <div id="red-star" class="star-type red">
                <span>Red Star: 0 (1/s)</span>
                <button onclick="buyStarType('red')">Buy (100 Stars)</button>
            </div>
            <div id="gold-star" class="star-type gold">
                <span>Gold Star: 0 (5/s)</span>
                <button onclick="buyStarType('gold')">Buy (500 Stars)</button>
            </div>
            <div id="cosmic-star" class="star-type cosmic">
                <span>Cosmic Star: 0 (20/s)</span>
                <button onclick="buyStarType('cosmic')">Buy (2000 Stars)</button>
            </div>
            <div id="nebula-star" class="star-type nebula">
                <span>Nebula Star: 0 (100/s)</span>
                <button onclick="buyStarType('nebula')">Buy (10000 Stars)</button>
            </div>
            <div id="pulsar-star" class="star-type pulsar">
                <span>Pulsar Star: 0 (500/s)</span>
                <button onclick="buyStarType('pulsar')">Buy (50000 Stars)</button>
            </div>
            <div id="supernova-star" class="star-type supernova">
                <span>Supernova Star: 0 (2500/s)</span>
                <button onclick="buyStarType('supernova')">Buy (250000 Stars)</button>
            </div>
            <div id="black-hole-star" class="star-type black-hole">
                <span>Black Hole Star: 0 (10000/s)</span>
                <button onclick="buyStarType('black-hole')">Buy (1000000 Stars)</button>
            </div>
        </div>
        <div id="upgrades">
            <h3>Upgrades</h3>
            <button id="upgrade-click" onclick="buyUpgrade('click')">Stronger Clicks (50 Stars)</button>
            <button id="upgrade-blue" onclick="buyUpgrade('blue')">Blue Star Glow (100 Stars)</button>
            <button id="upgrade-red" onclick="buyUpgrade('red')">Red Star Heat (500 Stars)</button>
            <button id="upgrade-gold" onclick="buyUpgrade('gold')">Gold Star Shine (2000 Stars)</button>
            <button id="upgrade-cosmic" onclick="buyUpgrade('cosmic')">Cosmic Star Pulse (8000 Stars)</button>
            <button id="upgrade-nebula" onclick="buyUpgrade('nebula')">Nebula Star Mist (40000 Stars)</button>
            <button id="upgrade-pulsar" onclick="buyUpgrade('pulsar')">Pulsar Star Flash (200000 Stars)</button>
            <button id="upgrade-supernova" onclick="buyUpgrade('supernova')">Supernova Star Burst (1000000 Stars)</button>
            <button id="upgrade-black-hole" onclick="buyUpgrade('black-hole')">Black Hole Star Gravity (5000000 Stars)</button>
            <button id="upgrade-galactic" onclick="buyUpgrade('galactic')">Galactic Boost (10000000 Stars)</button>
        </div>
    </div>
    <div id="save-load">
        <button onclick="saveGame()">Save Game</button>
        <button onclick="loadGame()">Load Game</button>
        <button onclick="resetGame()">Reset Game</button>
    </div>

    <script>
        let stars = 0;
        let clickValue = 1;
        let starTypes = {
            blue: { count: 0, cost: 15, baseProduction: 0.1, multiplier: 1 },
            red: { count: 0, cost: 100, baseProduction: 1, multiplier: 1 },
            gold: { count: 0, cost: 500, baseProduction: 5, multiplier: 1 },
            cosmic: { count: 0, cost: 2000, baseProduction: 20, multiplier: 1 },
            nebula: { count: 0, cost: 10000, baseProduction: 100, multiplier: 1 },
            pulsar: { count: 0, cost: 50000, baseProduction: 500, multiplier: 1 },
            supernova: { count: 0, cost: 250000, baseProduction: 2500, multiplier: 1 },
            'black-hole': { count: 0, cost: 1000000, baseProduction: 10000, multiplier: 1 }
        };
        let upgrades = {
            click: { purchased: false, cost: 50, effect: () => clickValue *= 2 },
            blue: { purchased: false, cost: 100, effect: () => starTypes.blue.multiplier *= 2 },
            red: { purchased: false, cost: 500, effect: () => starTypes.red.multiplier *= 2 },
            gold: { purchased: false, cost: 2000, effect: () => starTypes.gold.multiplier *= 2 },
            cosmic: { purchased: false, cost: 8000, effect: () => starTypes.cosmic.multiplier *= 2 },
            nebula: { purchased: false, cost: 40000, effect: () => starTypes.nebula.multiplier *= 2 },
            pulsar: { purchased: false, cost: 200000, effect: () => starTypes.pulsar.multiplier *= 2 },
            supernova: { purchased: false, cost: 1000000, effect: () => starTypes.supernova.multiplier *= 2 },
            'black-hole': { purchased: false, cost: 5000000, effect: () => starTypes['black-hole'].multiplier *= 2 },
            galactic: { purchased: false, cost: 10000000, effect: () => {
                for (let type in starTypes) starTypes[type].multiplier *= 2;
            }}
        };

        const starElement = document.getElementById('star');
        const starCountElement = document.getElementById('star-count');
        const starsPerSecondElement = document.getElementById('stars-per-second');

        // Click to collect stars
        starElement.addEventListener('click', () => {
            stars += clickValue;
            updateDisplay();
        });

        // Buy star type
        function buyStarType(type) {
            if (stars >= starTypes[type].cost) {
                stars -= starTypes[type].cost;
                starTypes[type].count += 1;
                starTypes[type].cost = Math.round(starTypes[type].cost * 1.15);
                updateDisplay();
            }
        }

        // Buy upgrade
        function buyUpgrade(type) {
            if (!upgrades[type].purchased && stars >= upgrades[type].cost) {
                stars -= upgrades[type].cost;
                upgrades[type].effect();
                upgrades[type].purchased = true;
                updateDisplay();
            }
        }

        // Calculate stars per second
        function getStarsPerSecond() {
            let total = 0;
            for (let type in starTypes) {
                total += starTypes[type].count * starTypes[type].baseProduction * starTypes[type].multiplier;
            }
            return total;
        }

        // Update production every second
        setInterval(() => {
            stars += getStarsPerSecond();
            updateDisplay();
        }, 1000);

        // Update display
        function updateDisplay() {
            starCountElement.textContent = `Stars: ${Math.floor(stars).toLocaleString()}`;
            starsPerSecondElement.textContent = `Stars per second: ${getStarsPerSecond().toFixed(1)}`;
            for (let type in starTypes) {
                document.getElementById(`${type}-star`).querySelector('span').textContent = 
                    `${type.replace('-', ' ').split(' ').map(w => w.charAt(0).toUpperCase() + w.slice(1)).join(' ')} Star: ${starTypes[type].count} (${(starTypes[type].baseProduction * starTypes[type].multiplier).toFixed(1)}/s)`;
                document.getElementById(`${type}-star`).querySelector('button').textContent = `Buy (${starTypes[type].cost.toLocaleString()} Stars)`;
                document.getElementById(`${type}-star`).querySelector('button').disabled = stars < starTypes[type].cost;
            }
            for (let upgrade in upgrades) {
                const button = document.getElementById(`upgrade-${upgrade}`);
                button.disabled = stars < upgrades[upgrade].cost || upgrades[upgrade].purchased;
                if (upgrades[upgrade].purchased) {
                    button.textContent = `${button.textContent.split(' (')[0]} (Purchased)`;
                }
            }
        }

        // Save game
        function saveGame() {
            const gameState = { stars, clickValue, starTypes, upgrades };
            localStorage.setItem('starCollectorSave', JSON.stringify(gameState));
            alert('Game saved!');
        }

        // Load game
        function loadGame() {
            const savedState = localStorage.getItem('starCollectorSave');
            if (savedState) {
                const gameState = JSON.parse(savedState);
                stars = gameState.stars;
                clickValue = gameState.clickValue;
                starTypes = gameState.starTypes;
                upgrades = gameState.upgrades;
                updateDisplay();
                alert('Game loaded!');
            } else {
                alert('No saved game found!');
            }
        }

        // Reset game
        function resetGame() {
            if (confirm('Are you sure you want to reset your progress?')) {
                stars = 0;
                clickValue = 1;
                starTypes = {
                    blue: { count: 0, cost: 15, baseProduction: 0.1, multiplier: 1 },
                    red: { count: 0, cost: 100, baseProduction: 1, multiplier: 1 },
                    gold: { count: 0, cost: 500, baseProduction: 5, multiplier: 1 },
                    cosmic: { count: 0, cost: 2000, baseProduction: 20, multiplier: 1 },
                    nebula: { count: 0, cost: 10000, baseProduction: 100, multiplier: 1 },
                    pulsar: { count: 0, cost: 50000, baseProduction: 500, multiplier: 1 },
                    supernova: { count: 0, cost: 250000, baseProduction: 2500, multiplier: 1 },
                    'black-hole': { count: 0, cost: 1000000, baseProduction: 10000, multiplier: 1 }
                };
                upgrades = {
                    click: { purchased: false, cost: 50, effect: () => clickValue *= 2 },
                    blue: { purchased: false, cost: 100, effect: () => starTypes.blue.multiplier *= 2 },
                    red: { purchased: false, cost: 500, effect: () => starTypes.red.multiplier *= 2 },
                    gold: { purchased: false, cost: 2000, effect: () => starTypes.gold.multiplier *= 2 },
                    cosmic: { purchased: false, cost: 8000, effect: () => starTypes.cosmic.multiplier *= 2 },
                    nebula: { purchased: false, cost: 40000, effect: () => starTypes.nebula.multiplier *= 2 },
                    pulsar: { purchased: false, cost: 200000, effect: () => starTypes.pulsar.multiplier *= 2 },
                    supernova: { purchased: false, cost: 1000000, effect: () => starTypes.supernova.multiplier *= 2 },
                    'black-hole': { purchased: false, cost: 5000000, effect: () => starTypes['black-hole'].multiplier *= 2 },
                    galactic: { purchased: false, cost: 10000000, effect: () => {
                        for (let type in starTypes) starTypes[type].multiplier *= 2;
                    }}
                };
                localStorage.removeItem('starCollectorSave');
                updateDisplay();
                alert('Game reset!');
            }
        }

        // Initial display update
        updateDisplay();
    </script>
</body>
</html>
