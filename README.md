<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>🌱 Pixel Garden Life</title>
    <style>
        /* ===== RESET & BASE ===== */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            background: #1a1a2e;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            font-family: 'Courier New', monospace;
            image-rendering: pixelated;
            padding: 16px;
        }

        /* ===== GAME WRAPPER ===== */
        .game-wrapper {
            background: #2d2d44;
            padding: 20px 24px 24px 24px;
            border-radius: 16px;
            border: 4px solid #4a4a6a;
            box-shadow: 0 8px 0 #0f0f1a, 0 12px 40px rgba(0, 0, 0, 0.7);
            max-width: 820px;
            width: 100%;
        }

        /* ===== HEADER / STATS ===== */
        .header {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 8px 16px;
            margin-bottom: 14px;
            background: #22223b;
            padding: 10px 16px;
            border-radius: 10px;
            border: 2px solid #3a3a5a;
            color: #e0e0e8;
        }

        .stat-group {
            display: flex;
            align-items: center;
            gap: 6px 14px;
            flex-wrap: wrap;
        }

        .stat {
            display: flex;
            align-items: center;
            gap: 4px;
            font-size: 15px;
            background: #2a2a44;
            padding: 2px 12px 2px 8px;
            border-radius: 20px;
            border: 1px solid #3f3f62;
        }

        .stat .icon {
            font-size: 18px;
            line-height: 1;
        }
        .stat .value {
            font-weight: bold;
            color: #fff;
            min-width: 20px;
            text-align: center;
        }

        .stat .label {
            color: #aab;
            font-size: 12px;
        }

        /* XP & Energy bars */
        .bar-wrap {
            display: flex;
            align-items: center;
            gap: 6px;
            background: #1a1a30;
            padding: 2px 10px 2px 6px;
            border-radius: 20px;
            border: 1px solid #3a3a5a;
        }

        .bar-wrap .label {
            font-size: 12px;
            color: #aab;
            min-width: 22px;
        }

        .bar-outer {
            width: 100px;
            height: 14px;
            background: #151528;
            border-radius: 8px;
            overflow: hidden;
            border: 1px solid #3a3a5a;
            position: relative;
        }

        .bar-inner {
            height: 100%;
            border-radius: 8px;
            transition: width 0.25s ease;
            background: linear-gradient(90deg, #f7d44a, #f5b342);
        }

        .bar-inner.energy {
            background: linear-gradient(90deg, #4fc3f7, #29b6f6);
        }

        .bar-text {
            position: absolute;
            right: 4px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 9px;
            color: #fff;
            text-shadow: 0 1px 2px rgba(0, 0, 0, 0.8);
            font-weight: bold;
        }

        /* ===== CANVAS ===== */
        .canvas-wrap {
            background: #1a1a2e;
            border-radius: 12px;
            border: 4px solid #3a3a5a;
            padding: 4px;
            box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.5);
            position: relative;
            margin-bottom: 12px;
        }

        canvas {
            display: block;
            width: 100%;
            height: auto;
            image-rendering: pixelated;
            background: #2d4a2d;
            border-radius: 8px;
            cursor: pointer;
            aspect-ratio: 16 / 12;
        }

        /* ===== BOTTOM BAR ===== */
        .footer {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 8px;
            padding: 8px 12px;
            background: #22223b;
            border-radius: 10px;
            border: 2px solid #3a3a5a;
            min-height: 44px;
        }

        .footer .info {
            color: #b8b8d0;
            font-size: 13px;
            flex: 1;
            min-width: 140px;
        }

        .footer .info .highlight {
            color: #f7d44a;
            font-weight: bold;
        }

        .footer .actions {
            display: flex;
            gap: 6px;
            flex-wrap: wrap;
        }

        .btn {
            background: #3a3a5a;
            border: none;
            border-radius: 8px;
            padding: 6px 14px;
            color: #e0e0e8;
            font-family: 'Courier New', monospace;
            font-size: 13px;
            font-weight: bold;
            cursor: pointer;
            border-bottom: 3px solid #1a1a30;
            transition: all 0.08s linear;
            image-rendering: pixelated;
        }

        .btn:hover {
            background: #4a4a6e;
            transform: translateY(-1px);
            border-bottom-width: 4px;
        }

        .btn:active {
            transform: translateY(2px);
            border-bottom-width: 1px;
        }

        .btn.gold {
            background: #b8942a;
            border-bottom-color: #7a5f1a;
            color: #1a1a2e;
        }

        .btn.gold:hover {
            background: #d4aa3a;
        }

        .btn.green {
            background: #3a8a4a;
            border-bottom-color: #1a5a2a;
            color: #fff;
        }

        .btn.green:hover {
            background: #4aaa5a;
        }

        .btn.red {
            background: #8a3a3a;
            border-bottom-color: #5a1a1a;
            color: #fff;
        }

        .btn.red:hover {
            background: #aa4a4a;
        }

        .btn.blue {
            background: #3a5a8a;
            border-bottom-color: #1a3a5a;
            color: #fff;
        }

        .btn.blue:hover {
            background: #4a6aaa;
        }

        .btn:disabled {
            opacity: 0.4;
            transform: translateY(0);
            border-bottom-width: 3px;
            cursor: not-allowed;
        }

        /* ===== MESSAGE POPUP ===== */
        .msg-popup {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #22223b;
            border: 4px solid #f7d44a;
            border-radius: 16px;
            padding: 24px 32px;
            color: #e0e0e8;
            font-size: 20px;
            text-align: center;
            box-shadow: 0 8px 0 #0f0f1a, 0 20px 60px rgba(0, 0, 0, 0.9);
            z-index: 100;
            min-width: 220px;
            max-width: 90vw;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.2s ease;
        }

        .msg-popup.show {
            opacity: 1;
            pointer-events: auto;
        }

        .msg-popup .sub {
            font-size: 14px;
            color: #aab;
            margin-top: 6px;
        }

        /* ===== RESPONSIVE ===== */
        @media (max-width: 640px) {
            .game-wrapper {
                padding: 12px 12px 16px 12px;
            }
            .header {
                padding: 8px 10px;
                flex-direction: column;
                align-items: stretch;
                gap: 4px;
            }
            .stat-group {
                justify-content: center;
            }
            .stat {
                font-size: 13px;
                padding: 1px 8px 1px 4px;
            }
            .bar-outer {
                width: 70px;
                height: 12px;
            }
            .footer {
                flex-direction: column;
                align-items: stretch;
                gap: 6px;
            }
            .footer .info {
                font-size: 12px;
                text-align: center;
            }
            .footer .actions {
                justify-content: center;
            }
            .btn {
                font-size: 12px;
                padding: 5px 10px;
            }
            .msg-popup {
                font-size: 16px;
                padding: 16px 20px;
                min-width: 160px;
            }
        }

        @media (max-width: 420px) {
            .stat .label {
                display: none;
            }
            .bar-outer {
                width: 50px;
            }
            .btn {
                font-size: 11px;
                padding: 4px 8px;
            }
        }
    </style>
</head>
<body>

    <!-- ===== GAME WRAPPER ===== -->
    <div class="game-wrapper">

        <!-- HEADER / STATS -->
        <div class="header">
            <div class="stat-group">
                <div class="stat">
                    <span class="icon">🧑‍🌾</span>
                    <span class="label">Lv.</span>
                    <span class="value" id="levelDisplay">1</span>
                </div>
                <div class="bar-wrap">
                    <span class="label">XP</span>
                    <div class="bar-outer">
                        <div class="bar-inner" id="xpBar" style="width:0%"></div>
                        <span class="bar-text" id="xpText">0/10</span>
                    </div>
                </div>
                <div class="bar-wrap">
                    <span class="label">⚡</span>
                    <div class="bar-outer">
                        <div class="bar-inner energy" id="energyBar" style="width:100%"></div>
                        <span class="bar-text" id="energyText">100</span>
                    </div>
                </div>
                <div class="stat">
                    <span class="icon">🪙</span>
                    <span class="label">Koin</span>
                    <span class="value" id="coinDisplay">5</span>
                </div>
            </div>
            <div class="stat-group">
                <div class="stat" style="background:#2a3a2a;border-color:#4a6a4a;">
                    <span class="icon">🏠</span>
                    <span class="label">Rumah</span>
                    <span class="value" id="houseDisplay">🌱</span>
                </div>
            </div>
        </div>

        <!-- CANVAS -->
        <div class="canvas-wrap">
            <canvas id="gameCanvas" width="640" height="480"></canvas>
        </div>

        <!-- FOOTER -->
        <div class="footer">
            <div class="info" id="infoText">
                🌱 <span class="highlight">Klik</span> tanah untuk menanam |
                <span class="highlight">Panen</span> tanaman matang |
                <span class="highlight">WASD</span> gerak
            </div>
            <div class="actions">
                <button class="btn green" id="btnRest">💤 Istirahat</button>
                <button class="btn gold" id="btnBuild">🔨 Bangun</button>
                <button class="btn blue" id="btnHelp">❓ Bantuan</button>
            </div>
        </div>
    </div>

    <!-- ===== MESSAGE POPUP ===== -->
    <div class="msg-popup" id="msgPopup">
        <div id="msgTitle">🌱 Selamat datang!</div>
        <div class="sub" id="msgSub">Klik di mana saja untuk mulai</div>
    </div>

    <!-- ============================================================ -->
    <!-- ===== JAVASCRIPT ===== -->
    <script>
        // ================================================================
        //  PIXEL GARDEN LIFE  –  Full Game
        // ================================================================

        // -------- CONSTANTS --------
        const COLS = 16;
        const ROWS = 12;
        const TILE_SIZE = 40; // px

        const PLANT_STAGES = ['seed', 'sprout', 'growing', 'mature'];
        const STAGE_NAMES = ['🌰 Benih', '🌱 Kecambah', '🌿 Tumbuh', '🍎 Matang'];

        const GROWTH_TIMES = [5, 8, 10]; // seconds per stage transition

        // -------- CANVAS SETUP --------
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = COLS * TILE_SIZE;
        canvas.height = ROWS * TILE_SIZE;

        // -------- GAME STATE --------
        const state = {
            // Player
            player: { x: 7, y: 5 },
            level: 1,
            xp: 0,
            xpNext: 10,
            energy: 100,
            maxEnergy: 100,
            coins: 5,

            // Garden grid: each tile is { type, stage, timer, elapsed, plantedAt, waterCount }
            grid: [],

            // House: 0=foundation, 1=walls, 2=complete, -1=empty
            houseStage: -1,
            houseBuilt: false,

            // Day/night cycle (0-1, 0=noon, 0.5=midnight)
            timeOfDay: 0.25,
            daySpeed: 0.0004, // per frame

            // UI
            selectedAction: 'plant', // 'plant' | 'water'
            message: '',
            messageTimer: 0,

            // Cooldown / growth
            frame: 0,
            lastTime: 0,
            gameOver: false,
        };

        // -------- INIT GRID --------
        function initGrid() {
            state.grid = [];
            for (let row = 0; row < ROWS; row++) {
                state.grid[row] = [];
                for (let col = 0; col < COLS; col++) {
                    // House area: top-left 3x3
                    const isHouseArea = (row < 3 && col < 3);
                    state.grid[row][col] = {
                        type: isHouseArea ? 'house_plot' : 'grass',
                        stage: -1, // -1 = no plant
                        timer: 0, // seconds remaining in current stage
                        elapsed: 0, // total growth time
                        waterCount: 0,
                        plantedAt: 0,
                        harvestable: false,
                    };
                }
            }
            // Set some initial dirt patches
            for (let i = 0; i < 12; i++) {
                const r = 3 + Math.floor(Math.random() * (ROWS - 4));
                const c = 3 + Math.floor(Math.random() * (COLS - 4));
                if (state.grid[r][c].type === 'grass') {
                    state.grid[r][c].type = 'dirt';
                }
            }
            // Ensure a few dirt tiles near player
            for (let dx = -1; dx <= 1; dx++) {
                for (let dy = -1; dy <= 1; dy++) {
                    const r = state.player.y + dy;
                    const c = state.player.x + dx;
                    if (r >= 0 && r < ROWS && c >= 0 && c < COLS && state.grid[r][c].type === 'grass') {
                        state.grid[r][c].type = 'dirt';
                    }
                }
            }
            // Clear house area
            for (let r = 0; r < 3; r++) {
                for (let c = 0; c < 3; c++) {
                    state.grid[r][c].type = 'house_plot';
                    state.grid[r][c].stage = -1;
                }
            }
        }

        // -------- HELPERS --------
        function getTile(r, c) {
            if (r < 0 || r >= ROWS || c < 0 || c >= COLS) return null;
            return state.grid[r][c];
        }

        function isAdjacent(pr, pc, tr, tc) {
            return Math.abs(pr - tr) <= 1 && Math.abs(pc - tc) <= 1 && !(pr === tr && pc === tc);
        }

        function clamp(v, min, max) { return Math.max(min, Math.min(max, v)); }

        function lerp(a, b, t) { return a + (b - a) * t; }

        function rand(min, max) { return min + Math.random() * (max - min); }

        function randInt(min, max) { return Math.floor(rand(min, max + 1)); }

        // -------- XP & LEVELING --------
        function xpForLevel(lv) { return Math.floor(10 + lv * 4 + lv * lv * 0.5); }

        function addXP(amount) {
            state.xp += amount;
            while (state.xp >= state.xpNext) {
                state.xp -= state.xpNext;
                state.level++;
                state.xpNext = xpForLevel(state.level);
                // Bonus on level up
                state.maxEnergy = Math.min(150, 100 + state.level * 4);
                state.energy = Math.min(state.energy + 20, state.maxEnergy);
                showMessage(`🎉 Level Up! Lv.${state.level}`, `Energi +20, Max +${state.level*4}`);
                // Update house bonus
                if (state.houseStage === 2) {
                    showMessage('🏠 Rumah lengkap! +2 koin per panen!', '');
                }
            }
            updateUI();
        }

        // -------- ENERGY --------
        function spendEnergy(amount) {
            if (state.energy < amount) return false;
            state.energy -= amount;
            updateUI();
            return true;
        }

        function restoreEnergy(amount) {
            state.energy = Math.min(state.energy + amount, state.maxEnergy);
            updateUI();
        }

        // -------- COINS --------
        function addCoins(amount) {
            state.coins += amount;
            updateUI();
        }

        function spendCoins(amount) {
            if (state.coins < amount) return false;
            state.coins -= amount;
            updateUI();
            return true;
        }

        // -------- PLANT LOGIC --------
        function plantSeed(row, col) {
            const tile = getTile(row, col);
            if (!tile) return false;
            if (tile.type !== 'dirt') return false;
            if (tile.stage >= 0) return false;
            if (!spendCoins(1)) {
                showMessage('💸 Tidak cukup koin!', 'Panen tanaman untuk dapat koin');
                return false;
            }
            if (!spendEnergy(2)) {
                // Refund coin if no energy
                addCoins(1);
                showMessage('⚡ Energi habis!', 'Istirahat dulu yuk');
                return false;
            }
            tile.type = 'plant';
            tile.stage = 0; // seed
            tile.timer = GROWTH_TIMES[0];
            tile.elapsed = 0;
            tile.waterCount = 0;
            tile.harvestable = false;
            tile.plantedAt = state.frame;
            showMessage(`🌰 Menanam benih!`, `Panen dalam ${GROWTH_TIMES[0]+GROWTH_TIMES[1]+GROWTH_TIMES[2]} detik`);
            updateUI();
            return true;
        }

        function waterPlant(row, col) {
            const tile = getTile(row, col);
            if (!tile) return false;
            if (tile.type !== 'plant') return false;
            if (tile.stage >= 3) return false; // mature doesn't need water
            if (tile.stage < 0) return false;
            if (!spendEnergy(2)) {
                showMessage('⚡ Energi habis!', 'Istirahat dulu yuk');
                return false;
            }
            tile.waterCount++;
            // Speed up growth: reduce timer by 1.5s per water (min 0.5s)
            const reduction = Math.min(1.5, tile.timer - 0.5);
            tile.timer -= reduction;
            showMessage(`💧 Disiram! (+${reduction.toFixed(1)}s percepat)`, `Air: ${tile.waterCount}x`);
            updateUI();
            return true;
        }

        function harvestPlant(row, col) {
            const tile = getTile(row, col);
            if (!tile) return false;
            if (tile.type !== 'plant') return false;
            if (tile.stage !== 3) {
                showMessage('🌱 Belum matang!', `Saatnya: ${STAGE_NAMES[tile.stage]}`);
                return false;
            }
            if (!tile.harvestable) {
                showMessage('⏳ Belum siap panen!', 'Tunggu sebentar lagi');
                return false;
            }
            if (!spendEnergy(2)) {
                showMessage('⚡ Energi habis!', 'Istirahat dulu yuk');
                return false;
            }

            // Harvest rewards
            const baseXP = 3 + Math.floor(state.level / 2);
            const baseCoins = 2 + Math.floor(state.level / 3);
            const bonus = state.houseStage === 2 ? 2 : 0; // house bonus
            const xpGain = baseXP + Math.floor(Math.random() * 2);
            const coinGain = baseCoins + bonus + Math.floor(Math.random() * 2);

            addXP(xpGain);
            addCoins(coinGain);

            // Reset tile to dirt
            tile.type = 'dirt';
            tile.stage = -1;
            tile.timer = 0;
            tile.elapsed = 0;
            tile.waterCount = 0;
            tile.harvestable = false;

            showMessage(`🍎 Panen! +${xpGain} XP, +${coinGain} Koin`, bonus > 0 ? `Bonus rumah +${bonus}!` : '');
            updateUI();
            return true;
        }

        // -------- HOUSE BUILDING --------
        function buildHouse() {
            if (state.houseStage === 2) {
                showMessage('🏠 Rumah sudah jadi!', 'Nikmati bonus +2 koin per panen');
                return false;
            }

            const costs = [10, 20, 30];
            const stage = state.houseStage + 1; // next stage (0,1,2)
            const cost = costs[stage] || 30;

            if (!spendCoins(cost)) {
                showMessage(`💸 Butuh ${cost} koin!`, `Tahap ${stage+1}/3`);
                return false;
            }

            state.houseStage = stage;
            if (stage === 2) {
                state.houseBuilt = true;
                showMessage('🏠 Rumah selesai! 🎉', '+2 koin ekstra setiap panen!');
            } else {
                const names = ['🏗️ Fondasi', '🧱 Dinding', '🏠 Rumah'];
                showMessage(`🔨 ${names[stage]} dibangun!`, `Tahap ${stage+1}/3 selesai`);
            }
            updateUI();
            return true;
        }

        // -------- PLAYER MOVEMENT --------
        function movePlayer(dx, dy) {
            const nx = state.player.x + dx;
            const ny = state.player.y + dy;
            if (nx < 0 || nx >= COLS || ny < 0 || ny >= ROWS) return false;
            // Can't walk into house plot if house is built? Actually you can walk on it.
            // But let's allow walking on house plot.
            if (!spendEnergy(1)) {
                showMessage('⚡ Energi habis!', 'Istirahat dulu yuk');
                return false;
            }
            state.player.x = nx;
            state.player.y = ny;
            updateUI();
            return true;
        }

        // -------- GROWTH UPDATE --------
        function updateGrowth(deltaSec) {
            for (let r = 0; r < ROWS; r++) {
                for (let c = 0; c < COLS; c++) {
                    const tile = state.grid[r][c];
                    if (tile.type !== 'plant') continue;
                    if (tile.stage < 0 || tile.stage >= 3) continue;

                    // Reduce timer
                    const speedMult = 1 + (state.level - 1) * 0.04; // 4% faster per level
                    tile.timer -= deltaSec * speedMult;

                    if (tile.timer <= 0) {
                        // Advance stage
                        tile.stage++;
                        if (tile.stage === 3) {
                            // Mature! Ready to harvest after a short wait
                            tile.harvestable = true;
                            tile.timer = 0;
                            showMessage(`🌾 Tanaman di (${c+1},${r+1}) matang!`, 'Klik untuk panen');
                        } else {
                            tile.timer = GROWTH_TIMES[tile.stage] || 10;
                            // Small visual feedback
                        }
                    }
                }
            }
        }

        // -------- DAY/NIGHT CYCLE --------
        function updateDayNight(deltaSec) {
            state.timeOfDay += state.daySpeed * deltaSec * 60;
            if (state.timeOfDay > 1) state.timeOfDay -= 1;
        }

        // -------- ENERGY REGEN --------
        function regenEnergy(deltaSec) {
            // Regen 1 energy per 2.5 seconds
            const rate = 1 / 2.5;
            const amount = rate * deltaSec;
            if (state.energy < state.maxEnergy) {
                state.energy = Math.min(state.energy + amount, state.maxEnergy);
                updateUI();
            }
        }

        // -------- UI UPDATE --------
        function updateUI() {
            document.getElementById('levelDisplay').textContent = state.level;
            document.getElementById('coinDisplay').textContent = state.coins;

            const xpPct = state.xpNext > 0 ? (state.xp / state.xpNext * 100) : 0;
            document.getElementById('xpBar').style.width = Math.min(xpPct, 100) + '%';
            document.getElementById('xpText').textContent = `${Math.floor(state.xp)}/${state.xpNext}`;

            const enPct = state.maxEnergy > 0 ? (state.energy / state.maxEnergy * 100) : 0;
            document.getElementById('energyBar').style.width = Math.min(enPct, 100) + '%';
            document.getElementById('energyText').textContent = Math.floor(state.energy);

            const houseEmojis = ['🏗️', '🧱', '🏠'];
            const houseLabel = state.houseStage >= 0 ? houseEmojis[state.houseStage] || '🏠' : '🌱';
            document.getElementById('houseDisplay').textContent = houseLabel;
        }

        // -------- MESSAGE SYSTEM --------
        let msgTimeout = null;

        function showMessage(title, sub = '') {
            const popup = document.getElementById('msgPopup');
            document.getElementById('msgTitle').textContent = title;
            document.getElementById('msgSub').textContent = sub;
            popup.classList.add('show');
            if (msgTimeout) clearTimeout(msgTimeout);
            msgTimeout = setTimeout(() => {
                popup.classList.remove('show');
            }, 2000);
        }

        // -------- RENDERING --------
        function drawPixelSprite(x, y, size, pixels) {
            // pixels: 2D array of color strings
            const px = size / pixels.length;
            for (let r = 0; r < pixels.length; r++) {
                for (let c = 0; c < pixels[r].length; c++) {
                    const color = pixels[r][c];
                    if (color) {
                        ctx.fillStyle = color;
                        ctx.fillRect(x + c * px, y + r * px, px + 0.5, px + 0.5);
                    }
                }
            }
        }

        function drawTile(row, col) {
            const x = col * TILE_SIZE;
            const y = row * TILE_SIZE;
            const tile = state.grid[row][col];

            // ---- Background ----
            // Grass or dirt
            if (tile.type === 'grass') {
                const shade = ((row + col) % 2 === 0) ? '#3d7a3d' : '#4a8a4a';
                ctx.fillStyle = shade;
                ctx.fillRect(x, y, TILE_SIZE, TILE_SIZE);
                // Tiny grass detail
                if ((row * 7 + col * 13) % 5 === 0) {
                    ctx.fillStyle = '#5a9a5a';
                    ctx.fillRect(x + 10, y + 28, 4, 6);
                    ctx.fillRect(x + 26, y + 30, 4, 6);
                }
            } else if (tile.type === 'dirt') {
                const shade = ((row + col) % 2 === 0) ? '#7a5a3a' : '#8a6a4a';
                ctx.fillStyle = shade;
                ctx.fillRect(x, y, TILE_SIZE, TILE_SIZE);
                // Dirt texture
                ctx.fillStyle = '#6a4a2a';
                ctx.fillRect(x + 6, y + 12, 5, 4);
                ctx.fillRect(x + 28, y + 30, 5, 4);
                ctx.fillRect(x + 16, y + 22, 4, 5);
            } else if (tile.type === 'house_plot') {
                // House area background
                const shade = ((row + col) % 2 === 0) ? '#5a5a4a' : '#6a6a5a';
                ctx.fillStyle = shade;
                ctx.fillRect(x, y, TILE_SIZE, TILE_SIZE);
            } else if (tile.type === 'plant') {
                // Dirt background for plants
                const shade = ((row + col) % 2 === 0) ? '#7a5a3a' : '#8a6a4a';
                ctx.fillStyle = shade;
                ctx.fillRect(x, y, TILE_SIZE, TILE_SIZE);
            }

            // ---- House ----
            if (tile.type === 'house_plot') {
                const stage = state.houseStage;
                if (stage >= 0) {
                    // House tile: draw based on stage and position
                    const hr = row,
                        hc = col;
                    // Only draw house parts on specific tiles
                    // Foundation: all 3x3 tiles get stone base
                    ctx.fillStyle = '#6a6a5a';
                    ctx.fillRect(x + 2, y + 30, 36, 8);
                    ctx.fillStyle = '#7a7a6a';
                    ctx.fillRect(x + 4, y + 32, 32, 6);

                    if (stage >= 1) {
                        // Walls: draw walls on edges and interior pillars
                        const isEdge = (hr === 0 || hr === 2 || hc === 0 || hc === 2);
                        if (isEdge) {
                            ctx.fillStyle = '#8a7a5a';
                            ctx.fillRect(x + 2, y + 4, 36, 28);
                            ctx.fillStyle = '#9a8a6a';
                            ctx.fillRect(x + 4, y + 6, 8, 24);
                            ctx.fillRect(x + 28, y + 6, 8, 24);
                            if (hr === 0 || hr === 2) {
                                ctx.fillRect(x + 12, y + 6, 16, 8);
                            }
                            // Window
                            if (hr === 0 && hc === 1) {
                                ctx.fillStyle = '#7ac4e8';
                                ctx.fillRect(x + 12, y + 12, 16, 12);
                                ctx.fillStyle = '#5a9ab8';
                                ctx.fillRect(x + 14, y + 14, 5, 8);
                                ctx.fillRect(x + 21, y + 14, 5, 8);
                            }
                            if (hr === 1 && hc === 0) {
                                ctx.fillStyle = '#7ac4e8';
                                ctx.fillRect(x + 12, y + 12, 16, 12);
                                ctx.fillStyle = '#5a9ab8';
                                ctx.fillRect(x + 14, y + 14, 5, 8);
                                ctx.fillRect(x + 21, y + 14, 5, 8);
                            }
                        } else {
                            // Interior floor
                            ctx.fillStyle = '#6a5a3a';
                            ctx.fillRect(x + 4, y + 4, 32, 32);
                        }
                    }

                    if (stage >= 2) {
                        // Roof: only on top row
                        if (hr === 0) {
                            ctx.fillStyle = '#b84a2a';
                            ctx.fillRect(x - 4, y - 6, 48, 12);
                            ctx.fillStyle = '#c85a3a';
                            ctx.fillRect(x - 2, y - 4, 44, 8);
                            // Roof tiles
                            for (let i = 0; i < 8; i++) {
                                ctx.fillStyle = (i % 2 === 0) ? '#b84a2a' : '#a83a1a';
                                ctx.fillRect(x - 2 + i * 6, y - 4, 6, 4);
                            }
                            // Chimney
                            if (hc === 2) {
                                ctx.fillStyle = '#6a5a4a';
                                ctx.fillRect(x + 26, y - 14, 10, 10);
                                ctx.fillStyle = '#7a6a5a';
                                ctx.fillRect(x + 28, y - 12, 6, 6);
                            }
                        }
                        // Door: on the bottom center tile
                        if (hr === 2 && hc === 1) {
                            ctx.fillStyle = '#5a3a2a';
                            ctx.fillRect(x + 10, y + 16, 20, 20);
                            ctx.fillStyle = '#7a5a3a';
                            ctx.fillRect(x + 12, y + 18, 8, 16);
                            ctx.fillRect(x + 20, y + 18, 8, 16);
                            ctx.fillStyle = '#d4a040';
                            ctx.fillRect(x + 18, y + 28, 4, 4); // doorknob
                        }
                    }
                }
                return; // house tiles done
            }

            // ---- Plants ----
            if (tile.type === 'plant' && tile.stage >= 0) {
                const stage = tile.stage;
                const cx = x + TILE_SIZE / 2;
                const cy = y + TILE_SIZE / 2 + 4;

                if (stage === 0) { // Seed
                    ctx.fillStyle = '#6a4a2a';
                    ctx.beginPath();
                    ctx.ellipse(cx, cy + 4, 6, 4, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.fillStyle = '#8a6a3a';
                    ctx.beginPath();
                    ctx.ellipse(cx - 2, cy + 3, 3, 2, 0, 0, Math.PI * 2);
                    ctx.fill();
                } else if (stage === 1) { // Sprout
                    ctx.fillStyle = '#5a8a3a';
                    ctx.fillRect(cx - 1, cy - 8, 2, 10);
                    ctx.fillStyle = '#6aaa4a';
                    ctx.fillRect(cx - 6, cy - 8, 5, 4);
                    ctx.fillRect(cx + 1, cy - 10, 5, 4);
                    ctx.fillStyle = '#5a9a3a';
                    ctx.fillRect(cx - 4, cy - 6, 3, 3);
                    ctx.fillRect(cx + 2, cy - 8, 3, 3);
                } else if (stage === 2) { // Growing
                    ctx.fillStyle = '#4a7a2a';
                    ctx.fillRect(cx - 2, cy - 16, 4, 18);
                    // Leaves
                    const leafPositions = [
                        [-8, -12],
                        [6, -14],
                        [-6, -6],
                        [8, -8],
                        [-10, -18],
                        [8, -20]
                    ];
                    for (const [lx, ly] of leafPositions) {
                        ctx.fillStyle = '#5aaa3a';
                        ctx.fillRect(cx + lx, cy + ly, 7, 5);
                        ctx.fillStyle = '#6aba4a';
                        ctx.fillRect(cx + lx + 1, cy + ly + 1, 5, 3);
                    }
                    // Stem detail
                    ctx.fillStyle = '#3a6a1a';
                    ctx.fillRect(cx - 1, cy - 8, 2, 6);
                } else if (stage === 3) { // Mature
                    // Tall plant with fruit/flower
                    ctx.fillStyle = '#4a7a2a';
                    ctx.fillRect(cx - 2, cy - 24, 4, 26);
                    // More leaves
                    const leafPositions = [
                        [-10, -18],
                        [8, -20],
                        [-8, -10],
                        [10, -12],
                        [-12, -24],
                        [10, -26],
                        [-4, -8],
                        [2, -8]
                    ];
                    for (const [lx, ly] of leafPositions) {
                        ctx.fillStyle = '#5aaa3a';
                        ctx.fillRect(cx + lx, cy + ly, 8, 5);
                        ctx.fillStyle = '#6aba4a';
                        ctx.fillRect(cx + lx + 1, cy + ly + 1, 6, 3);
                    }
                    // Fruit / flower
                    const fruitColor = ['#e84a4a', '#f0a030', '#d060d0', '#4ac8e8'][(row + col) % 4];
                    ctx.fillStyle = fruitColor;
                    ctx.beginPath();
                    ctx.ellipse(cx, cy - 28, 8, 7, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.fillStyle = '#fff8e0';
                    ctx.beginPath();
                    ctx.ellipse(cx - 3, cy - 30, 3, 2, 0, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.fillStyle = '#ff6a6a';
                    ctx.beginPath();
                    ctx.ellipse(cx + 3, cy - 26, 2, 2, 0, 0, Math.PI * 2);
                    ctx.fill();

                    // Harvest indicator (sparkle)
                    if (tile.harvestable) {
                        const pulse = Math.sin(state.frame * 0.08) * 0.3 + 0.7;
                        ctx.fillStyle = `rgba(255, 215, 0, ${pulse*0.4})`;
                        ctx.fillRect(cx - 14, cy - 34, 28, 10);
                        ctx.fillStyle = `rgba(255, 255, 200, ${pulse*0.6})`;
                        ctx.fillRect(cx - 10, cy - 32, 20, 6);
                        // Little star
                        ctx.fillStyle = `rgba(255, 255, 180, ${pulse})`;
                        for (let i = 0; i < 4; i++) {
                            const angle = state.frame * 0.05 + i * Math.PI / 2;
                            const sx = cx + Math.cos(angle) * 14;
                            const sy = cy - 26 + Math.sin(angle) * 10;
                            ctx.fillRect(sx - 2, sy - 2, 4, 4);
                        }
                    }
                }
            }

            // ---- Player ----
            if (row === state.player.y && col === state.player.x) {
                const px = x + 4;
                const py = y + 4;
                // Body
                ctx.fillStyle = '#4a8ad4';
                ctx.fillRect(px + 6, py + 12, 20, 16);
                ctx.fillRect(px + 4, py + 16, 24, 8);
                // Head
                ctx.fillStyle = '#f5d0a0';
                ctx.fillRect(px + 8, py + 2, 16, 14);
                ctx.fillRect(px + 6, py + 6, 20, 6);
                // Hat
                ctx.fillStyle = '#6a4a2a';
                ctx.fillRect(px + 4, py, 24, 6);
                ctx.fillRect(px + 8, py - 4, 16, 6);
                // Eyes
                ctx.fillStyle = '#2a2a3a';
                ctx.fillRect(px + 10, py + 6, 4, 4);
                ctx.fillRect(px + 18, py + 6, 4, 4);
                ctx.fillStyle = '#fff';
                ctx.fillRect(px + 11, py + 7, 2, 2);
                ctx.fillRect(px + 19, py + 7, 2, 2);
                // Smile
                ctx.fillStyle = '#c04040';
                ctx.fillRect(px + 13, py + 12, 6, 2);
                // Legs
                ctx.fillStyle = '#3a5a8a';
                ctx.fillRect(px + 8, py + 28, 6, 6);
                ctx.fillRect(px + 18, py + 28, 6, 6);
                // Shoes
                ctx.fillStyle = '#3a2a1a';
                ctx.fillRect(px + 6, py + 32, 10, 4);
                ctx.fillRect(px + 16, py + 32, 10, 4);
                // Watering can indicator
                if (state.selectedAction === 'water') {
                    ctx.fillStyle = '#4ac8e8';
                    ctx.fillRect(px + 26, py + 8, 6, 10);
                    ctx.fillRect(px + 28, py + 16, 2, 4);
                }
            }
        }

        function render() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // ---- Draw all tiles ----
            for (let r = 0; r < ROWS; r++) {
                for (let c = 0; c < COLS; c++) {
                    drawTile(r, c);
                }
            }

            // ---- Day/night overlay ----
            const tod = state.timeOfDay;
            const darkness = Math.sin(tod * Math.PI * 2) * 0.35 + 0.35;
            if (darkness > 0.1) {
                ctx.fillStyle = `rgba(10, 10, 30, ${darkness * 0.5})`;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                // Stars
                if (darkness > 0.3) {
                    for (let i = 0; i < 20; i++) {
                        const sx = (i * 137 + 42) % canvas.width;
                        const sy = (i * 251 + 13) % canvas.height;
                        const bright = Math.sin(i * 73 + state.frame * 0.02) * 0.3 + 0.7;
                        ctx.fillStyle = `rgba(255, 255, 240, ${bright * darkness * 0.6})`;
                        ctx.fillRect(sx, sy, 2, 2);
                    }
                }
            }

            // ---- Grid lines (subtle) ----
            ctx.strokeStyle = 'rgba(255,255,255,0.04)';
            ctx.lineWidth = 1;
            for (let r = 1; r < ROWS; r++) {
                ctx.beginPath();
                ctx.moveTo(0, r * TILE_SIZE);
                ctx.lineTo(canvas.width, r * TILE_SIZE);
                ctx.stroke();
            }
            for (let c = 1; c < COLS; c++) {
                ctx.beginPath();
                ctx.moveTo(c * TILE_SIZE, 0);
                ctx.lineTo(c * TILE_SIZE, canvas.height);
                ctx.stroke();
            }

            // ---- Time indicator ----
            const hour = Math.floor(tod * 24);
            const minute = Math.floor((tod * 24 - hour) * 60);
            ctx.fillStyle = 'rgba(255,255,255,0.15)';
            ctx.font = '10px monospace';
            ctx.fillText(`${String(hour).padStart(2,'0')}:${String(minute).padStart(2,'0')}`, 4, 14);
        }

        // -------- GAME LOOP --------
        function gameLoop(timestamp) {
            const delta = state.lastTime ? (timestamp - state.lastTime) / 1000 : 0;
            state.lastTime = timestamp;
            state.frame++;

            // Clamp delta to avoid jumps
            const dt = Math.min(delta, 0.1);

            // Update
            updateGrowth(dt);
            updateDayNight(dt);
            regenEnergy(dt);

            // Render
            render();
            updateUI();

            requestAnimationFrame(gameLoop);
        }

        // -------- INPUT HANDLING --------
        // Keyboard
        document.addEventListener('keydown', (e) => {
            const key = e.key;
            // Prevent scrolling
            if (['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight', ' '].includes(key)) {
                e.preventDefault();
            }

            let dx = 0,
                dy = 0;
            if (key === 'ArrowUp' || key === 'w' || key === 'W') dy = -1;
            else if (key === 'ArrowDown' || key === 's' || key === 'S') dy = 1;
            else if (key === 'ArrowLeft' || key === 'a' || key === 'A') dx = -1;
            else if (key === 'ArrowRight' || key === 'd' || key === 'D') dx = 1;
            else if (key === ' ') {
                // Interact: plant or harvest on current tile
                const r = state.player.y,
                    c = state.player.x;
                const tile = getTile(r, c);
                if (!tile) return;
                if (tile.type === 'dirt' && state.selectedAction === 'plant') {
                    plantSeed(r, c);
                } else if (tile.type === 'plant' && tile.stage === 3 && tile.harvestable) {
                    harvestPlant(r, c);
                } else if (tile.type === 'plant' && tile.stage >= 0 && tile.stage < 3) {
                    waterPlant(r, c);
                } else if (tile.type === 'house_plot') {
                    buildHouse();
                } else {
                    showMessage('👀 Tidak ada yang bisa dilakukan', 'Coba di tanah atau tanaman');
                }
                return;
            } else if (key === 'r' || key === 'R') {
                restPlayer();
                return;
            }

            if (dx !== 0 || dy !== 0) {
                movePlayer(dx, dy);
            }
        });

        // Mouse click on canvas
        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            const mx = (e.clientX - rect.left) * scaleX;
            const my = (e.clientY - rect.top) * scaleY;
            const col = Math.floor(mx / TILE_SIZE);
            const row = Math.floor(my / TILE_SIZE);
            if (row < 0 || row >= ROWS || col < 0 || col >= COLS) return;

            const tile = getTile(row, col);
            if (!tile) return;

            // Check adjacency
            const pr = state.player.y,
                pc = state.player.x;
            const adj = (Math.abs(pr - row) <= 1 && Math.abs(pc - col) <= 1);

            if (!adj) {
                showMessage('🚶 Mendekat dulu!', `Posisi player (${pc+1},${pr+1})`);
                return;
            }

            // Action based on tile
            if (tile.type === 'dirt' && state.selectedAction === 'plant') {
                plantSeed(row, col);
            } else if (tile.type === 'plant' && tile.stage === 3 && tile.harvestable) {
                harvestPlant(row, col);
            } else if (tile.type === 'plant' && tile.stage >= 0 && tile.stage < 3) {
                waterPlant(row, col);
            } else if (tile.type === 'house_plot') {
                buildHouse();
            } else if (tile.type === 'grass') {
                // Convert grass to dirt
                if (spendEnergy(2)) {
                    tile.type = 'dirt';
                    showMessage('🌾 Tanah digarap!', 'Siap untuk ditanami');
                    updateUI();
                } else {
                    showMessage('⚡ Energi habis!', 'Istirahat dulu yuk');
                }
            } else {
                showMessage('👀 Tidak ada yang bisa dilakukan', 'Coba di tanah atau tanaman');
            }
        });

        // -------- BUTTONS --------
        document.getElementById('btnRest').addEventListener('click', restPlayer);

        function restPlayer() {
            if (state.energy >= state.maxEnergy) {
                showMessage('💤 Energi sudah penuh!', 'Segar bugar!');
                return;
            }
            const restore = 25 + Math.floor(state.level * 2);
            restoreEnergy(restore);
            showMessage(`💤 Istirahat... +${restore} energi`, `⚡ ${Math.floor(state.energy)}/${state.maxEnergy}`);
        }

        document.getElementById('btnBuild').addEventListener('click', () => {
            // Build house by clicking the house plot
            const hr = 1,
                hc = 1; // center of house area
            const tile = getTile(hr, hc);
            if (tile && tile.type === 'house_plot') {
                // Try to build, but require player to be adjacent
                const pr = state.player.y,
                    pc = state.player.x;
                if (Math.abs(pr - hr) <= 1 && Math.abs(pc - hc) <= 1) {
                    buildHouse();
                } else {
                    showMessage('🚶 Mendekat ke rumah!', `Posisi di (${pc+1},${pr+1})`);
                }
            } else {
                showMessage('🏠 Area rumah di pojok kiri atas', 'Mendekat lalu klik atau tekan Bangun');
            }
        });

        document.getElementById('btnHelp').addEventListener('click', () => {
            showMessage(
                '🌱 Pixel Garden Life',
                'WASD: gerak | Klik: tanam/panen/siram | Spasi: aksi | R: istirahat | Bangun: buat rumah'
            );
        });

        // Toggle action with 'p' key
        document.addEventListener('keydown', (e) => {
            if (e.key === 'p' || e.key === 'P') {
                state.selectedAction = state.selectedAction === 'plant' ? 'water' : 'plant';
                const label = state.selectedAction === 'plant' ? '🌱 Tanam' : '💧 Siram';
                showMessage(`🔧 Mode: ${label}`, 'Klik pada tile yang sesuai');
                updateUI();
            }
        });

        // -------- START GAME --------
        initGrid();
        updateUI();
        showMessage('🌱 Pixel Garden Life', 'WASD gerak | Klik tanah untuk tanam | Spasi aksi');

        // Start game loop
        requestAnimationFrame(gameLoop);

        // ================================================================
        //  END GAME
        // ================================================================
    </script>
</body>
</html>
