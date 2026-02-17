+++
title = "Games"
description = "Fun puzzle games by dhruvik"
date = "2023-12-25"
aliases = ["games", "puzzle", "fun"]
author = "Dhruvik Donga"
+++

> Logic is a muscle. These puzzles are the gym. Created for the moments between the code, to keep the mind sharp and the spirit curious.

{{< notice tip >}}
<div style="font-size: 1.3rem; line-height: 1.5;">

**Did you know?** Engaging in logical puzzles stimulates neuroplasticity—essentially "freshening" your 🧠 cells.

Do share with your friends and help them kill some time productively. 🚀
</div>

<hr style="margin: 15px 0; opacity: 0.2;">

<div style="font-size: 1.2rem; line-height: 1.3; opacity: 0.8;">
    📊 <i>Analytics are processed client-side via localStorage. Your data is your own—no cookies, no tracking, just pure performance metrics.</i><br>
    <a href="javascript:void(0)" onclick="toggleAnalytics()" id="view-analytics-link" style="color: var(--success-color); text-decoration: underline; margin-right: 10px;">View Your Game Analytics</a>
    <a href="javascript:void(0)" onclick="clearAllStats()" style="color: var(--error-color); text-decoration: underline;">Clear all local stats</a>
</div>
<div id="analytics-section" style="display: none; margin-top: 20px; padding: 20px; background: #424242; border: 1px solid #424242; border-radius: 8px; color: #c9d1d9;">
    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
        <select id="chart-game-select" class="game-btn p-size-btn" onchange="renderHistoryChart()" style="font-size: 1.2rem; padding: 4px; background: #424242; color: #ffffff; border: 1px solid #424242;">
            <option value="nonogram">Nonogram</option>
            <option value="minesweeper">Minesweeper</option>
            <option value="binaryLogic">Binary Sudoku</option>
            <option value="2048">2048 (High Tile)</option>
            <option value="pathFinder">Path Finder</option>
        </select>
        <button onclick="toggleAnalytics()" style="background: none; border: none; color: #ffffff; cursor: pointer; font-size: 1.2rem;">Close ✖</button>
    </div>
    <div style="height: 250px; width: 100%; background: #424242; border-radius: 6px; padding: 10px; border: 1px solid #424242;">
        <canvas id="historyChart"></canvas>
    </div>   
    <hr style="margin: 15px 0; opacity: 0.2;">
    <div id="funny-highlights" style="display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 10px; margin-bottom: 10px;margin-top: 10px;">
    </div>
    <details style="cursor: pointer;"> 
<div style="font-size: 1.4rem; line-height: 1.6; margin-top: 15px; color: #8b949e;">

### 📊 Analytics: The Data Pipeline
- Since I'm into Cloud Services, I built a localized "Data Warehouse".
- **Ingestion**: **StatsManager** pushes JSON payloads to **localStorage** on every "Win."
- **Processing**: **getHighlights** performs aggregate functions (Sum, Avg) to determine your "Turtle Mode."
- **Visualization**: **Chart.js** maps your learning curve against time.

***The site isn't mad you're slow, just disappointed. 📈***

</div> 
</details>

</div>
{{< /notice >}}

<div class="game-index-container" style="margin-bottom: 20px;">
    <select class="game-btn" onchange="location.hash = this.value; this.selectedIndex = 0;" style="width: 100%; max-width: 300px; background: #424242; border-color: #424242; font-size: 1.5rem;">
        <option value="" disabled selected>Jump to Game...</option>
        <option value="#nonogram">Nonogram</option>
        <option value="#minesweeper">Minesweeper</option>
        <option value="#binary-sudoku">Binary Sudoku</option>
        <option value="#2048">2048</option>
        <option value="#path-finder">Path Finder</option>
    </select>
</div>

<style>
    html {
        scroll-behavior: smooth;
    }
    :root {
        --text-color: #c9d1d9;
        --cell-bg: #2f4f4f;
        --cell-filled: #1f6feb;
        --cell-crossed: #8b949e;
        --cell-revealed: #161b22;
        --cell-mine: #f85149;
        --hover-bg: #21262d;
        --success-color: #238636;
        --btn-bg: #21262d;
        --btn-border: #363b42;
        --border-color: #30363d;
    }

    /* Shared Component Styles */
    .game-section { width: 100%; margin-bottom: 3rem; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif; }
    .controls, .tools, .timer-container { margin-bottom: 1rem; display: flex; gap: 8px; flex-wrap: wrap; }
    .game-btn { background-color: var(--btn-bg); color: var(--text-color); border: 1px solid var(--btn-border); padding: 5px 16px; cursor: pointer; border-radius: 6px; font-size: 14px; font-weight: 500; transition: 0.2s; }
    .game-btn:hover { background-color: #30363d; border-color: #8b949e; }
    .game-btn.active { background-color: #2f4f4f; color: #ffffff; }
    .timer { font-size: 2rem; font-family: ui-monospace, SFMono-Regular, monospace; }
    .seed-info { font-size: 12px; color: #8b949e; margin-top: 5px; }

    /* Nonogram Specific */
    .n-grid { display: grid; gap: 2px; background-color: var(--border-color); border: 1px solid var(--border-color); padding: 2px; border-radius: 4px; width: max-content; line-height: 0; }
    .n-header { background-color: #0d1117; display: flex; flex-direction: column; justify-content: flex-end; align-items: center; padding: 4px; font-size: 12px; font-family: ui-monospace, monospace; color: #8b949e; line-height: 1.2; min-width: 30px; }
    .n-header.satisfied { color: var(--success-color); }
    .n-row-h { flex-direction: row; justify-content: flex-end; padding-right: 8px; min-height: 30px; }
    .n-cell { background-color: var(--cell-bg); cursor: pointer; width: 30px; height: 30px; display: flex; justify-content: center; align-items: center; user-select: none; transition: background-color 0.4s ease; }
    .n-cell.filled { background-color: var(--cell-filled); }
    .n-cell.crossed::after { content: "×"; color: var(--cell-crossed); font-size: 1.5rem; }
    .victory-animation .n-cell.filled { background-color: var(--success-color) !important; }

    /* Minesweeper Specific */
    .m-grid { display: grid; gap: 2px; background-color: var(--border-color); border: 1px solid var(--border-color); padding: 2px; border-radius: 4px; width: max-content; line-height: 0; }
    .m-cell { background-color: var(--cell-bg); cursor: pointer; width: 30px; height: 30px; display: flex; justify-content: center; align-items: center; user-select: none; font-family: ui-monospace, monospace; font-weight: bold; font-size: 14px; line-height: 1; }
    .m-cell.revealed { background-color: var(--cell-revealed); cursor: default; }
    .m-cell.mine { background-color: var(--cell-mine); }
    .m-cell.flagged::after { content: "🚩"; font-size: 12px; }
    .num-1 { color: #58a6ff; } .num-2 { color: #3fb950; } .num-3 { color: #f85149; }

    /* Binary Sudoku */
    .b-grid { display: grid; gap: 2px; background-color: var(--border-color); border: 1px solid var(--border-color); padding: 2px; border-radius: 4px; width: max-content; line-height: 0; }
    .b-cell { background-color: var(--cell-bg); cursor: pointer; width: 40px; height: 40px; display: flex; justify-content: center; align-items: center; user-select: none; font-family: ui-monospace, monospace; font-weight: bold; font-size: 18px; color: var(--text-color); }
    .b-cell.fixed { background-color: #0d1117; color: #58a6ff; cursor: default; }
    .b-cell.invalid { color: var(--cell-mine); }

    /* Path Finder */
    .p-grid { 
        display: grid; gap: 2px; background-color: var(--border-color); 
        border: 1px solid var(--border-color); padding: 2px; 
        border-radius: 4px; width: max-content; 
        touch-action: none; /* Prevents scrolling while drawing on mobile */
    }
    .p-cell { 
        background-color: var(--cell-bg); width: 35px; height: 35px; 
        display: flex; justify-content: center; align-items: center; 
        user-select: none; font-family: ui-monospace, monospace; font-weight: bold; 
    }
    .p-cell.wall { background-color: #0d1117; cursor: not-allowed; }
    .p-cell.start { background-color: #1f6feb; color: white; }
    .p-cell.end { background-color: #f85149; color: white; }
    .p-cell.path { background-color: var(--success-color); color: white; transition: background-color 0.2s; }

    /* 2048 Grid Styles */
    .t-container {
        position: relative; width: 320px; height: 320px;
        background: var(--grid-bg); border-radius: 8px;
        margin: 20px auto; padding: 10px; touch-action: none;
    }
    .t-grid-background {
        display: grid; grid-template-columns: repeat(4, 70px);
        grid-template-rows: repeat(4, 70px); gap: 10px;
    }
    .t-grid-cell { background: rgba(22, 27, 34, 0.5); border-radius: 4px; width: 70px; height: 70px; }
    
    .t-tile-layer { position: absolute; top: 10px; left: 10px; width: 300px; height: 300px; pointer-events: none; }
    
    .t-tile {
        position: absolute; width: 70px; height: 70px;
        background: var(--tile-bg); border-radius: 4px;
        display: flex; justify-content: center; align-items: center;
        font-weight: bold; font-size: 1.5rem; color: #fff;
        /* THE ANIMATION ENGINE */
        transition: transform 0.3s ease-in-out, background-color 0.2s;
        z-index: 10;
    }
    .t-2 { background: #58a6ff; color: #0d1117; } .t-4 { background: #3fb950; color: #0d1117; }
    .t-8 { background: #d29922; } .t-16 { background: #f85149; } .t-32 { background: #ab7df8; }

    @keyframes pop { 0% { transform: scale(1); } 50% { transform: scale(1.15); } 100% { transform: scale(1); } }
    .t-merged { animation: pop 0.4s ease-in-out; }
    
</style>

## Nonogram
---
💡 [How to solve a nonogram](https://www.youtube.com/watch?v=zisu0Qf4TAI)  
💡 You can click the size options to generate a fresh game anytime.
<details style="cursor: pointer;"> 
<summary style="font-size: 1.2rem; font-weight: bold; color: var(--success-color);">🛠️ Technical Deep Dive: How the "Game" Works</summary>

<div style="font-size: 1.4rem; line-height: 1.6; color: #8b949e;">

### 🧩 Nonogram: The Constraint Solver
- This is essentially a Constraint Satisfaction Problem (CSP).
- **The Algorithm**: Uses a Linear Congruential Generator (LCG) to create a stable, seeded board.
- **The Logic**: The engine scans rows and columns for contiguous filled blocks to generate "Hints."
- **Victory**: Runs a matrix comparison on every click.

*It’s like a Sudoku had a baby with a QR code. 🕵️‍♂️*
</div> 
</details>
<br>
<div class="game-section" id="nonogram-wrapper" style="display: flex; flex-direction: column; align-items: center; text-align: center;">
    <div class="controls">
        <button class="game-btn n-size-btn" id="n-btn-5" onclick="startNonogram(5)">5x5</button>
        <button class="game-btn n-size-btn" id="n-btn-8" onclick="startNonogram(8)">8x8</button>
        <button class="game-btn n-size-btn" id="n-btn-10" onclick="startNonogram(10)">10x10</button>
    </div>
    <div class="tools">
        <button class="game-btn active" id="n-tool-fill" onclick="setNTool(0)">Fill ■</button>
        <button class="game-btn" id="n-tool-cross" onclick="setNTool(2)">Cross ×</button>
    </div>
    <div class="timer-container">
        <div id="n-timer" class="timer">00:00</div>
        <div id="n-seed" class="seed-info"></div>
    </div>
    <div id="n-board" class="n-grid"></div>
    <div id="n-status" style="color: var(--success-color); font-weight: bold; margin-top: 1rem; min-height: 1.5em;"></div>
    <div id="nonogram-badges" class="badge-container"></div>
</div>


## Minesweeper
---
💡 [How to solve a minesweeper](https://www.youtube.com/shorts/Zil7a1hetQE)  
💡 You can click the size options to generate a fresh game anytime.
💡 **How to play:** Left-click to reveal a cell. Right-click or long press on mobile device to toggle a flag (🚩) on suspected mines.
<details style="cursor: pointer;"> 
<summary style="font-size: 1.2rem; font-weight: bold; color: var(--success-color);">🛠️ Technical Deep Dive: How the "Game" Works</summary>

<div style="font-size: 1.4rem; line-height: 1.6; color: #8b949e;">

### 💣 Minesweeper: The Recursive Flood
- Classic Minesweeper logic using the **Flood Fill** algorithm.
- **The Algorithm**: Triggers a Depth-First Search (DFS) to reveal all adjacent empty (0) tiles.
- **Safe Start**: The engine reserves a "Safe Zone" around your first click, moving mines elsewhere so you don't 💥 immediately.
    
*You’re a digital bomb technician with a flag as your only armor. 🚩*
</div>
</details>
<br>
<div class="game-section" id="mines-wrapper" style="display: flex; flex-direction: column; align-items: center; text-align: center;">
    <div class="controls">
        <button class="game-btn m-size-btn" id="m-btn-8" onclick="startMines(8, 10)">Easy (8x8)</button>
        <button class="game-btn m-size-btn" id="m-btn-12" onclick="startMines(12, 20)">Medium (12x12)</button>
    </div>
    <div class="timer-container">
        <div id="m-timer" class="timer">00:00</div>
        <div id="m-seed" class="seed-info"></div>
    </div>
    <div id="m-board" class="m-grid"></div>
    <div id="m-status" style="font-weight: bold; margin-top: 1rem; min-height: 1.5em;"></div>
    <div id="minesweeper-badges" class="badge-container"></div>
</div>

## Binary Sudoku
---
💡 Fill the grid with 0s and 1s. No more than two same numbers adjacent. Each row/col has equal 0s and 1s.
<details style="cursor: pointer;"> 
<summary style="font-size: 1.2rem; font-weight: bold; color: var(--success-color);">🛠️ Technical Deep Dive: How the "Game" Works</summary>

<div style="font-size: 1.4rem; line-height: 1.6; color: #8b949e;">

### 0️⃣1️⃣ Binary Sudoku: The Logic Gate
- This is a test of parity and adjacency rules.
- The Algorithm: The generator shuffles valid permutations of 0s and 1s while enforcing three strict rules: No more than two identical numbers adjacent, equal count of 0s/1s per row, and no duplicate rows.
- The Setup: It reveals ~30% of the solution to give you a "fixed" starting point.
- Victory Condition: A full sweep of the grid to ensure no rule violations exist (The "Logic Verify").

It's like trying to keep two siblings (0 and 1) from sitting next to each other for too long. 🧒↔️🧒
</div>
</details>
<br>
<div class="game-section" id="binary-wrapper" style="display: flex; flex-direction: column; align-items: center; text-align: center;">
    <div class="controls">
        <button class="game-btn b-size-btn" id="b-btn-4" onclick="startBinary(4)">4x4</button>
        <button class="game-btn b-size-btn" id="b-btn-6" onclick="startBinary(6)">6x6</button>
        <button class="game-btn b-size-btn" id="b-btn-8" onclick="startBinary(8)">8x8</button>
    </div>
    <div class="timer-container">
        <div id="b-timer" class="timer">00:00</div>
    </div>
    <div id="b-board" class="b-grid"></div>
    <div id="b-status" style="color: var(--success-color); font-weight: bold; margin-top: 1rem; min-height: 1.5em;"></div>
    <div id="binaryLogic-badges" class="badge-container"></div>
</div>

## 2048
---
<details style="cursor: pointer;"> 
<summary style="font-size: 1.2rem; font-weight: bold; color: var(--success-color);">🛠️ Technical Deep Dive: How the "Game" Works</summary>
<div style="font-size: 1.4rem; line-height: 1.6; color: #8b949e;">

### 🗺️ 2048: Matrix game
- Uses **Matrix Rotation** and **Array Compaction**. 
- Every move is treated as a "Slide Left" by rotating the grid state.
</div>
</details>

<div class="game-section" id="2048-wrapper" style="display: flex; flex-direction: column; align-items: center; text-align: center;">
    <button class="game-btn m-size-btn" onclick="init2048()">Reset Game 🔄</button>
    <div class="timer-container" style="margin-top: 5px">
        <div id="t-score" class="timer">Score: 0</div>
    </div>
    <div class="t-container" id="t-board-container">
        <div class="t-grid-background">
            <div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div>
            <div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div>
            <div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div>
            <div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div><div class="t-grid-cell"></div>
        </div>
        <div class="t-tile-layer" id="t-tile-layer"></div>
    </div>
    <p style="color: #8b949e; font-size: 1.2rem;">Swipe, Drag, or use Arrow Keys to merge!</p>
</div>

## Path Finder
---
<details style="cursor: pointer;"> 
<summary style="font-size: 1.2rem; font-weight: bold; color: var(--success-color);">🛠️ Technical Deep Dive: How the "Game" Works</summary>

<div style="font-size: 1.4rem; line-height: 1.6; color: #8b949e;">

### 🗺️ Path Finder: The BFS Navigator
- This is the "heavy lifter," using real-world routing logic.
- **The Algorithm**: Uses **BFS (Breadth-First Search)** to validate maze solvability.
- **The Maze**: The engine generates walls at ~55% density and runs a simulation. If the path isn't "complex" enough, it trashes the board and starts over.

*It’s like Google Maps, but I’ve replaced the roads with walls. 📍*
</div>
</details>
<br>
<div class="game-section" id="path-wrapper" style="display: flex; flex-direction: column; align-items: center; text-align: center;">
    <div class="controls">
        <button class="game-btn p-size-btn" id="p-btn-8" onclick="startPath(8)">8x8</button>
        <button class="game-btn p-size-btn" id="p-btn-10" onclick="startPath(10)">10x10</button>
        <button class="game-btn" onclick="undoPath()">Undo Last ⤺</button>
    </div>
    <div class="timer-container">
        <div id="p-timer" class="timer">00:00</div>
    </div>
    <div id="p-board" class="p-grid" onmouseleave="stopDrawing()"></div>
    <div id="p-status" style="color: var(--success-color); font-weight: bold; margin-top: 1rem; min-height: 1.5em;"></div>
    <div id="pathFinder-badges" class="badge-container"></div>
</div>

<button id="back-to-top" title="Go to top" onclick="scrollToTop()" style="
    display: none; 
    position: fixed; 
    bottom: 60px; 
    right: 20px; 
    z-index: 99; 
    border: 1px solid #424242; 
    outline: none; 
    background-color: #424242; 
    color: #ffffff; 
    cursor: pointer; 
    border-radius: 6px; 
    font-size: 15px;
    transition: opacity 0.3s;
    width: 3rem;
    height: 3rem;
">⬆</button>

<script>
    const StatsManager = {
        // Save details to localStorage
        saveGame(gameName, size, timeInSeconds) {
            let stats = JSON.parse(localStorage.getItem('dhruvik_game_stats')) || {};
            let history = JSON.parse(localStorage.getItem('dhruvik_game_history')) || [];
            
            // 1. Update Aggregated Stats (for Badges)
            if (!stats[gameName]) stats[gameName] = { totalTime: 0, totalCount: 0, segments: {} };
            const g = stats[gameName];
            g.totalCount++;
            g.totalTime += timeInSeconds;

            if (!g.segments[size]) g.segments[size] = { count: 0, bestTime: Infinity };
            const s = g.segments[size];
            s.count++;
            if (timeInSeconds < s.bestTime) s.bestTime = timeInSeconds;

            // 2. Update History (for Graphs)
            history.push({
                game: gameName,
                size: size,
                time: timeInSeconds,
                date: new Date().toLocaleString()
            });

            localStorage.setItem('dhruvik_game_stats', JSON.stringify(stats));
            localStorage.setItem('dhruvik_game_history', JSON.stringify(history));
            
            this.renderBadges(gameName);
            if(window.updateChart) window.updateChart(); // Refresh chart if it's open
            renderHighlights();
        },

        // Generate Shields.io badges
        renderBadges(gameName) {
            const stats = JSON.parse(localStorage.getItem('dhruvik_game_stats')) || {};
            const g = stats[gameName];
            const container = document.getElementById(`${gameName}-badges`);
            if (!g || !container) return;

            let badgeHTML = '';
            
            // Total Games Badge
            badgeHTML += `<img src="https://img.shields.io/badge/Total_Played-${g.totalCount}-blue?style=flat-square" /> `;

            // Segment Specific Badges (e.g., 8x8, 10x10)
            Object.keys(g.segments).forEach(size => {
                const s = g.segments[size];
                const timeFormat = this.formatTime(s.bestTime)
                const best = s.bestTime === Infinity ? 'N/A' : `${timeFormat}s`;

                // Badge for count in this segment
                badgeHTML += `<img src="https://img.shields.io/badge/${size}x${size}_Played-${s.count}-green?style=flat-square" /> `;
                // Badge for best time in this segment
                badgeHTML += `<img src="https://img.shields.io/badge/${size}x${size}_Best-${best}-orange?style=flat-square" /> `;
            });

            container.innerHTML = badgeHTML;
        },
        formatTime(seconds) {
            if (seconds === Infinity || isNaN(seconds) || seconds === null) return 'N/A';
            
            const units = [
                { label: 'mo', seconds: 30 * 24 * 60 * 60 },
                { label: 'd', seconds: 24 * 60 * 60 },
                { label: 'h', seconds: 60 * 60 },
                { label: 'm', seconds: 60 },
                { label: 's', seconds: 1 }
            ];

            let remaining = Math.floor(seconds);
            let parts = [];

            for (const unit of units) {
                if (remaining >= unit.seconds) {
                    const value = Math.floor(remaining / unit.seconds);
                    remaining %= unit.seconds;
                    parts.push(`${value}${unit.label}`);
                }
            }

            // Return the formatted string, or '0s' if the value was 0
            return parts.length > 0 ? parts.join(' ') : '0s';
        },
        getHighlights() {
            const stats = JSON.parse(localStorage.getItem('dhruvik_game_stats')) || {};
            const history = JSON.parse(localStorage.getItem('dhruvik_game_history')) || [];
            if (history.length === 0) return [];

            const highlights = [];

            // 1. Total Games Played
            let totalGames = Object.values(stats).reduce((acc, curr) => acc + curr.totalCount, 0);
            highlights.push({ 
                title: "Total Played", 
                val: `${totalGames} Games`, 
                icon: "🎮",
                info: "Total number of completed puzzle sessions across all games."
            });

            // 2. Player Rank
            let rank = "Novice", info = "Play more to rank up! (Architect: 10+, Expert: 25+, Grandmaster: 50+)", rankColor = "#8b949e";
            if (totalGames > 50) { rank = "Grandmaster"; rankColor = "#f85149"; }
            else if (totalGames > 25) { rank = "Expert"; rankColor = "#d29922"; }
            else if (totalGames > 10) { rank = "Architect"; rankColor = "#3fb950"; }
            
            highlights.push({ 
                title: "Player Rank", 
                val: rank, 
                icon: "🏆",
                color: rankColor,
                info: info
            });

            // 3. Overall Time Spent
            let totalSeconds = Object.values(stats).reduce((acc, curr) => acc + curr.totalTime, 0);
            highlights.push({ 
                title: "Overall Time", 
                val: this.formatTime(totalSeconds), 
                icon: "⏳",
                info: "The cumulative time spent solving puzzles on this device."
            });

            // 4. Fastest vs. Slowest
            let averages = Object.keys(stats).map(name => ({
                name: name,
                avg: stats[name].totalTime / stats[name].totalCount
            }));

            if (averages.length > 1) {
                let fastest = averages.reduce((a, b) => a.avg < b.avg ? a : b);
                let slowest = averages.reduce((a, b) => a.avg > b.avg ? a : b);
                
                highlights.push({ 
                    title: "Fastest At", 
                    val: fastest.name.charAt(0).toUpperCase() + fastest.name.slice(1), 
                    icon: "🚀",
                    info: `Based on your lowest average solve time: ${this.formatTime(fastest.avg)}.`
                });
                highlights.push({ 
                    title: "Turtle Mode", 
                    val: slowest.name.charAt(0).toUpperCase() + slowest.name.slice(1), 
                    icon: "🐢",
                    info: `Based on your highest average solve time: ${this.formatTime(slowest.avg)}.`
                });
            }

            return highlights;
        }
    };
    
    let myChart = null;

    function renderHistoryChart() {
        const game = document.getElementById('chart-game-select').value;
        const history = JSON.parse(localStorage.getItem('dhruvik_game_history')) || [];
        
        // Filter data for the specific game
        const gameData = history.filter(h => h.game === game);
        
        // Group unique sizes to create separate lines (e.g., 8x8 line, 10x10 line)
        const segments = [...new Set(gameData.map(h => h.size))];
        
        const datasets = segments.map((size, idx) => {
            const colors = ['#58a6ff', '#3fb950', '#f85149', '#d29922'];
            // Filter history for this specific size
            const sessionData = gameData.filter(h => h.size === size);
            
            return {
                label: `${size}x${size} Grid`,
                data: sessionData.map(h => h.time),
                borderColor: colors[idx % colors.length],
                backgroundColor: colors[idx % colors.length] + '33',
                tension: 0.2,
                pointRadius: 4,
                // --- CURVE SETTINGS ---
                tension: 0.4,       
                pointRadius: 4,
                pointHoverRadius: 6,
            };
        });

        const canvas = document.getElementById('historyChart');
        if (!canvas) return;
        const ctx = canvas.getContext('2d');
        
        if (myChart) myChart.destroy();
        myChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: Array.from({length: Math.max(...datasets.map(d => d.data.length), 0)}, (_, i) => i + 1),
                datasets: datasets
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: { 
                        beginAtZero: true,
                        title: { display: true, text: '⏰ Time', color: '#ffff' },
                        grid: { color: '#424242' }, // Dark grid lines
                        ticks: { 
                            color: '#ffff',
                            callback: function(value) {
                                return StatsManager.formatTime(value);
                            }
                        }
                    },
                    x: { 
                        title: { display: true, text: '🧩 Game Number', color: '#ffff' },
                        grid: { color: '#424242' }, // Dark grid lines
                        ticks: { color: '#ffff' }  // Light gray numbers
                    }
                },
                plugins: {
                    legend: { 
                        labels: { color: '#ffff' } // Light gray legend text
                    },
                    tooltip: {
                        backgroundColor: '#161b22',
                        titleColor: '#58a6ff',
                        bodyColor: '#c9d1d9',
                        borderColor: '#30363d',
                        borderWidth: 1,
                        callbacks: {
                            label: function(context) {
                                let label = context.dataset.label || '';
                                if (label) label += ': ';
                                if (context.parsed.y !== null) {
                                    label += StatsManager.formatTime(context.parsed.y);
                                }
                                return label;
                            }
                        }
                    }
                }
            }
        });
    }
    function toggleAnalytics() {
       const section = document.getElementById('analytics-section');
        const link = document.getElementById('view-analytics-link');
        if (!section || !link) return;

        if (section.style.display === 'none') {
            section.style.display = 'block';
            link.textContent = 'Hide Your Game Analytics';
            setTimeout(() => { renderHistoryChart();renderHighlights(); }, 50);
        } else {
            section.style.display = 'none';
            link.textContent = 'View Your Game Analytics';
        }
    }

    function clearAllStats() {
        if (confirm("Reset all game records?")) {
            localStorage.removeItem('dhruvik_game_stats');
            // Instantly clear the badges from view
            document.querySelectorAll('.badge-container').forEach(el => el.innerHTML = '');
        }
    }

    //Linear Congruential Generator (LCG).
    class SeededRNG {
        constructor(seed) {
            this.m = 0x80000000; this.a = 1103515245; this.c = 12345;
            this.state = seed ? seed : Math.floor(Math.random() * (this.m - 1));
        }
        nextInt() { this.state = (this.a * this.state + this.c) % this.m; return this.state; }
        nextFloat() { return this.nextInt() / (this.m - 1); }
    }

    // --- NONOGRAM LOGIC ---
    let nSize, nSol, nPlayer, nSeed, nTimer, nStartTime, nTool = 0, nStarted = false;

    function startNonogram(size) {
        nSize = size; 
        nStarted = false; 
        clearInterval(nTimer);
        document.getElementById('n-timer').textContent = "00:00";
        document.getElementById('n-status').textContent = "";
        document.getElementById('nonogram-wrapper').classList.remove('victory-animation');
        
        nSeed = Date.now();
        document.getElementById('n-seed').textContent = "Seed: " + nSeed;
        const rng = new SeededRNG(nSeed);
        
        document.querySelectorAll('.n-size-btn').forEach(b => b.classList.remove('active'));
        document.getElementById('n-btn-'+size).classList.add('active');

        // Initialize board state
        nSol = Array(size).fill().map(() => Array(size).fill().map(() => rng.nextFloat() > 0.5));
        nPlayer = Array(size).fill().map(() => Array(size).fill(0));
        
        renderNBoard();
    }

    function renderNBoard() {
        const board = document.getElementById('n-board');
        // Absolute pixel sizing (30px) prevents the grid from collapsing on larger 8x8+ sizes
        board.style.gridTemplateColumns = `max-content repeat(${nSize}, 30px)`;
        board.innerHTML = '';
        
        const corner = document.createElement('div'); 
        corner.className = 'n-header'; 
        board.appendChild(corner);

        const calcHints = (line) => {
            let h = [], c = 0;
            line.forEach(v => { if(v) c++; else if(c>0){ h.push(c); c=0; } });
            if(c>0) h.push(c); return h.length ? h : [0];
        };

        // Column Headers
        for(let c=0; c<nSize; c++) {
            const h = calcHints(nSol.map(r => r[c]));
            const pCol = nPlayer.map(r => r[c] === 1);
            const sat = JSON.stringify(h) === JSON.stringify(calcHints(pCol));
            const div = document.createElement('div'); 
            div.className = 'n-header' + (sat ? ' satisfied' : '');
            div.innerHTML = h.join('<br>'); 
            board.appendChild(div);
        }

        // Row Headers & Cells
        for(let r=0; r<nSize; r++) {
            const h = calcHints(nSol[r]);
            const pRow = nPlayer[r].map(v => v === 1);
            const sat = JSON.stringify(h) === JSON.stringify(calcHints(pRow));
            const head = document.createElement('div'); 
            head.className = 'n-header n-row-h' + (sat ? ' satisfied' : '');
            head.textContent = h.join(' '); 
            board.appendChild(head);

            for(let c=0; c<nSize; c++) {
                const cell = document.createElement('div'); 
                cell.className = 'n-cell';
                if(nPlayer[r][c] === 1) cell.classList.add('filled');
                if(nPlayer[r][c] === 2) cell.classList.add('crossed');
                
                cell.onmousedown = (e) => {
                    e.preventDefault();
                    handleNInput(r, c, e.button === 2 ? 2 : nTool);
                };
                cell.oncontextmenu = e => e.preventDefault();
                board.appendChild(cell);
            }
        }
    }

    function handleNInput(r, c, tool) {
        // Stop if already solved
        if(document.getElementById('n-status').textContent !== "") return;

        // Timer initialization
        if(!nStarted) { 
            nStarted = true; 
            nStartTime = Date.now(); 
            nTimer = setInterval(() => {
                let s = Math.floor((Date.now() - nStartTime) / 1000);
                document.getElementById('n-timer').textContent = 
                    Math.floor(s/60).toString().padStart(2,'0') + ":" + (s%60).toString().padStart(2,'0');
            }, 1000); 
        }

        const currentVal = nPlayer[r][c];
        // Ensure Fill=0 maps to state 1, and Cross=2 maps to state 2
        let targetState = (tool === 0) ? 1 : 2; 

        if (currentVal === targetState) {
            nPlayer[r][c] = 0; // Toggle off
        } else {
            nPlayer[r][c] = targetState; // Toggle on / Overwrite
        }

        renderNBoard();
        
        // Use a small timeout to ensure the DOM has rendered the latest satisfied classes
        // before checking the final win condition
        setTimeout(checkNWin, 10);
    }

    function checkNWin() {
        // A Nonogram is solved when all hint headers have the 'satisfied' class.
        // This is more reliable for 8x8+ boards than direct matrix comparison.
        const allHeaders = document.querySelectorAll('.n-header:not(:first-child)');
        const satisfiedHeaders = document.querySelectorAll('.n-header.satisfied');

        // Matrix fallback check
        let matrixMatch = true;
        for(let r=0; r<nSize; r++) {
            for(let c=0; c<nSize; c++) {
                if(nSol[r][c] !== (nPlayer[r][c] === 1)) {
                    matrixMatch = false;
                    break;
                }
            }
            if(!matrixMatch) break;
        }

        // Win if either the matrix matches OR all hint headers are satisfied
        if(matrixMatch || (allHeaders.length > 0 && allHeaders.length === satisfiedHeaders.length)) {
            document.getElementById('n-status').textContent = "Puzzle Solved!";
            document.getElementById('nonogram-wrapper').classList.add('victory-animation');
            const timeTaken = Math.floor((Date.now() - nStartTime) / 1000);
            StatsManager.saveGame('nonogram', nSize, timeTaken);
            clearInterval(nTimer);
        }
    }

    function setNTool(t) { 
        nTool = t; 
        document.getElementById('n-tool-fill').classList.toggle('active', t===0); 
        document.getElementById('n-tool-cross').classList.toggle('active', t===2); 
    }

    // --- MINESWEEPER LOGIC ---
    let mSize, mGrid, mRev, mFlag, mTimer, mStart, mAct = false, mOver = false;

    function startMines(size, mines) {
        mSize = size; mOver = false; mAct = false; clearInterval(mTimer);
        document.getElementById('m-timer').textContent = "00:00";
        document.getElementById('m-status').textContent = "";
        let seed = Date.now();
        document.getElementById('m-seed').textContent = "Seed: " + seed;
        const rng = new SeededRNG(seed);

        document.querySelectorAll('.m-size-btn').forEach(b => b.classList.remove('active'));
        document.getElementById('m-btn-'+size).classList.add('active');

        mGrid = Array(size).fill().map(() => Array(size).fill(0));
        mRev = Array(size).fill().map(() => Array(size).fill(false));
        mFlag = Array(size).fill().map(() => Array(size).fill(false));

        // Choose a random guaranteed start cell (not on edges for better numbers)
        const startR = 1 + Math.floor(rng.nextFloat() * (size - 2));
        const startC = 1 + Math.floor(rng.nextFloat() * (size - 2));

        let p = 0; 
        while(p < mines) {
            let r = Math.floor(rng.nextFloat()*size), c = Math.floor(rng.nextFloat()*size);
            // Avoid the start cell and its immediate neighbors to ensure a "numbered" start
            const isSafeZone = Math.abs(r - startR) <= 1 && Math.abs(c - startC) <= 1;
            if(mGrid[r][c] !== 'M' && !isSafeZone) { 
                mGrid[r][c] = 'M'; 
                p++; 
            }
        }

        for(let r=0; r<size; r++) {
            for(let c=0; c<size; c++) {
                if(mGrid[r][c] === 'M') continue;
                let count = 0;
                for(let i=-1; i<=1; i++) for(let j=-1; j<=1; j++) {
                    if(r+i>=0 && r+i<size && c+j>=0 && c+j<size && mGrid[r+i][c+j]==='M') count++;
                }
                mGrid[r][c] = count;
            }
        }
        renderMBoard();
        // Auto-reveal the chosen start cell to give the user a numbered block immediately
        revealM(startR, startC); 
    }

    function renderMBoard() {
        const board = document.getElementById('m-board');
        board.style.gridTemplateColumns = `repeat(${mSize}, 30px)`;
        board.innerHTML = '';
        for(let r=0; r<mSize; r++) {
            for(let c=0; c<mSize; c++) {
                const cell = document.createElement('div');
                cell.className = 'm-cell';
                
                if(mRev[r][c]) {
                    cell.classList.add('revealed');
                    if(mGrid[r][c]==='M') {
                        cell.classList.add('mine');
                        cell.textContent = '💣';
                    } else if(mGrid[r][c]>0) {
                        cell.textContent = mGrid[r][c];
                        cell.classList.add('num-'+mGrid[r][c]);
                    }
                } else if(mFlag[r][c]) {
                    cell.classList.add('flagged');
                }

                // --- MOBILE & DESKTOP UNIFIED LOGIC ---
                let touchTimer;
                
                // Desktop Right-Click
                cell.onmousedown = (e) => {
                    if(mOver) return;
                    initTimer();
                    if(e.button === 2) {
                        mFlag[r][c] = !mFlag[r][c];
                        renderMBoard();
                    } else {
                        revealM(r, c);
                    }
                };

                // Mobile Long-Press for Flagging
                cell.ontouchstart = (e) => {
                    if(mOver) return;
                    initTimer();
                    touchTimer = setTimeout(() => {
                        mFlag[r][c] = !mFlag[r][c];
                        renderMBoard();
                        touchTimer = null;
                    }, 500); // 500ms long press to flag
                };

                cell.ontouchend = (e) => {
                    if(touchTimer) {
                        clearTimeout(touchTimer);
                        revealM(r, c); // Normal tap reveals
                    }
                };

                cell.oncontextmenu = e => e.preventDefault();
                board.appendChild(cell);
            }
        }
    }

    // Helper to keep the timer logic dry
    function initTimer() {
        if(!mAct) {
            mAct = true;
            mStart = Date.now();
            mTimer = setInterval(() => {
                let s = Math.floor((Date.now()-mStart)/1000);
                document.getElementById('m-timer').textContent = 
                    Math.floor(s/60).toString().padStart(2,'0')+":"+(s%60).toString().padStart(2,'0');
            }, 1000);
        }
    }

    function revealM(r, c) {
        if(mRev[r][c] || mFlag[r][c]) return;
        mRev[r][c] = true;
        if(mGrid[r][c] === 'M') { 
            mOver = true; clearInterval(mTimer); document.getElementById('m-status').textContent = "GAME OVER";
            document.getElementById('m-status').style.color = "var(--cell-mine)";
            for(let ri=0; ri<mSize; ri++) for(let ci=0; ci<mSize; ci++) if(mGrid[ri][ci]==='M') mRev[ri][ci]=true;
        } else if(mGrid[r][c] === 0) {
            for(let i=-1; i<=1; i++) for(let j=-1; j<=1; j++) if(r+i>=0 && r+i<mSize && c+j>=0 && c+j<mSize) revealM(r+i, c+j);
        }
        renderMBoard();
        let revC = 0; mRev.forEach(row => row.forEach(v => {if(v) revC++}));
        let totalMines = (mSize === 8) ? 10 : 20;
        if(revC === (mSize*mSize) - totalMines && !mOver) {
            const timeTaken = Math.floor((Date.now() - mStart) / 1000);
            StatsManager.saveGame('minesweeper', mSize, timeTaken);
            mOver = true; clearInterval(mTimer); document.getElementById('m-status').textContent = "VICTORY!";
            document.getElementById('m-status').style.color = "var(--success-color)";
        }
    }

    // --- BINARY SUDOKU LOGIC ---
    let bSize, bSol, bPlayer, bFixed, bTimer, bStart, bStarted = false;
    function startBinary(size) {
        bSize = size; bStarted = false; clearInterval(bTimer);
        document.getElementById('b-timer').textContent = "00:00";
        document.getElementById('b-status').textContent = "";
        const rng = new SeededRNG(Date.now());
        document.querySelectorAll('.b-size-btn').forEach(b => b.classList.remove('active'));
        document.getElementById('b-btn-'+size).classList.add('active');
        
        // Generate valid solution (simplistic shuffle for 4,6,8)
        bSol = generateBinarySolution(size, rng);
        bPlayer = Array(size).fill().map(() => Array(size).fill(null));
        bFixed = Array(size).fill().map(() => Array(size).fill(false));

        // Reveal ~30% of cells
        for(let i=0; i<size*size*0.35; i++) {
            let r = Math.floor(rng.nextFloat()*size);
            let c = Math.floor(rng.nextFloat()*size);
            bPlayer[r][c] = bSol[r][c];
            bFixed[r][c] = true;
        }
        renderBinaryBoard();
    }

    function generateBinarySolution(size, rng) {
        let grid = Array(size).fill().map(() => Array(size).fill(0));
        // Fill rows with equal 0/1
        for(let r=0; r<size; r++) {
            let vals = [...Array(size/2).fill(0), ...Array(size/2).fill(1)];
            for (let i = vals.length - 1; i > 0; i--) {
                const j = Math.floor(rng.nextFloat() * (i + 1));
                [vals[i], vals[j]] = [vals[j], vals[i]];
            }
            grid[r] = vals;
        }
        return grid;
    }

    function renderBinaryBoard() {
        const board = document.getElementById('b-board');
        board.style.gridTemplateColumns = `repeat(${bSize}, 40px)`;
        board.innerHTML = '';
        for(let r=0; r<bSize; r++) {
            for(let c=0; c<bSize; c++) {
                const cell = document.createElement('div');
                cell.className = 'b-cell' + (bFixed[r][c] ? ' fixed' : '');
                cell.textContent = bPlayer[r][c] === null ? '' : bPlayer[r][c];
                if(!bFixed[r][c]) {
                    cell.onclick = () => handleBinaryInput(r, c);
                }
                board.appendChild(cell);
            }
        }
    }

    function handleBinaryInput(r, c) {
        if(document.getElementById('b-status').textContent) return;
        if(!bStarted) { bStarted = true; bStart = Date.now(); bTimer = setInterval(() => {
            let s = Math.floor((Date.now()-bStart)/1000);
            document.getElementById('b-timer').textContent = Math.floor(s/60).toString().padStart(2,'0')+":"+(s%60).toString().padStart(2,'0');
        }, 1000); }
        
        let val = bPlayer[r][c];
        if(val === null) bPlayer[r][c] = 0;
        else if(val === 0) bPlayer[r][c] = 1;
        else bPlayer[r][c] = null;
        
        renderBinaryBoard();
        checkBinaryWin();
    }

    function checkBinaryWin() {
        // Rules check
        for(let i=0; i<bSize; i++) {
            let row = bPlayer[i], col = bPlayer.map(r => r[i]);
            if(row.includes(null) || col.includes(null)) return;
            if(row.filter(v=>v===0).length !== bSize/2 || col.filter(v=>v===0).length !== bSize/2) return;
            for(let j=0; j<bSize-2; j++) {
                if(row[j]!==null && row[j]===row[j+1] && row[j]===row[j+2]) return;
                if(col[j]!==null && col[j]===col[j+1] && col[j]===col[j+2]) return;
            }
        }
        document.getElementById('b-status').textContent = "Logic Verified! Puzzle Solved.";
        const timeTaken = Math.floor((Date.now() - bStart) / 1000);
            StatsManager.saveGame('binaryLogic', bSize, timeTaken);
        clearInterval(bTimer);
    }

    // --- PATH FINDER LOGIC ---
    let pSize, pGrid, pPath = [], pTimer, pStart, pActive = false, pSolved = false, isDrawing = false;

    function startPath(size) {
        pSize = size; pSolved = false; pActive = false; pPath = []; isDrawing = false;
        clearInterval(pTimer);
        document.getElementById('p-timer').textContent = "00:00";
        document.getElementById('p-status').textContent = "";
        
        const rng = new SeededRNG(Date.now());
        document.querySelectorAll('.p-size-btn').forEach(b => b.classList.remove('active'));
        document.getElementById('p-btn-'+size).classList.add('active');

        let attempts = 0;
        while (attempts < 1000) {
            let wallDensity = (size === 8) ? 0.60 : 0.65; 
            
            let tempGrid = Array(size).fill().map(() => Array(size).fill(0));
            
            for(let r=0; r<size; r++) {
                for(let c=0; c<size; c++) {
                    // Ensure a small 1-cell buffer around S and E is more likely to be clear
                    if((r<=1 && c<=1) || (r>=size-2 && c>=size-2)) continue;
                    if(rng.nextFloat() < wallDensity) tempGrid[r][c] = 'W';
                }
            }
            tempGrid[0][0] = 'S'; 
            tempGrid[size-1][size-1] = 'E';

            let pathDist = getShortestPathDist(size, tempGrid);
            
            // Validation: Solvable but requires at least 'size * 2' steps to ensure difficulty
            if (pathDist >= (size * 2)) {
                pGrid = tempGrid;
                break; 
            }
            attempts++;
        }
        
        if (!pGrid) {
            pGrid = Array(size).fill().map(() => Array(size).fill(0));
            pGrid[0][0] = 'S'; pGrid[size-1][size-1] = 'E';
        }
        renderPathBoard();
    }

    function getShortestPathDist(size, grid) {
        let queue = [{r: 0, c: 0, d: 0}];
        let visited = new Set(['0,0']);
        
        while (queue.length > 0) {
            let {r, c, d} = queue.shift();
            if (r === size - 1 && c === size - 1) return d;

            let neighbors = [[0,1], [0,-1], [1,0], [-1,0]];
            for(let [dr, dc] of neighbors) {
                let nr = r + dr, nc = c + dc;
                let key = `${nr},${nc}`;
                if (nr >= 0 && nr < size && nc >= 0 && nc < size && 
                    grid[nr][nc] !== 'W' && !visited.has(key)) {
                    visited.add(key);
                    queue.push({r: nr, c: nc, d: d + 1});
                }
            }
        }
        return -1; // Not solvable
    }

    function renderPathBoard() {
        const board = document.getElementById('p-board');
        board.style.gridTemplateColumns = `repeat(${pSize}, 35px)`;
        board.innerHTML = '';

        for(let r=0; r<pSize; r++) {
            for(let c=0; c<pSize; c++) {
                const cell = document.createElement('div');
                cell.className = 'p-cell';
                if(pGrid[r][c] === 'W') cell.classList.add('wall');
                else if(pGrid[r][c] === 'S') { cell.classList.add('start'); cell.textContent = 'S'; }
                else if(pGrid[r][c] === 'E') { cell.classList.add('end'); cell.textContent = 'E'; }
                
                if(pPath.some(pos => pos.r === r && pos.c === c)) cell.classList.add('path');

                // Drag-to-draw mouse events
                cell.onmousedown = () => { if(!pSolved) { isDrawing = true; handlePathInput(r, c); } };
                cell.onmouseenter = () => { if(isDrawing) handlePathInput(r, c); };
                cell.onmouseup = () => stopDrawing();
                
                // Mobile Touch support
                cell.ontouchstart = (e) => { e.preventDefault(); isDrawing = true; handlePathInput(r, c); };
                cell.ontouchmove = (e) => {
                    e.preventDefault();
                    let touch = e.touches[0];
                    let target = document.elementFromPoint(touch.clientX, touch.clientY);
                    if(target && target.classList.contains('p-cell')) {
                        // Extract coords from a data attribute or search logic
                        // For simplicity, we trigger the click logic if it's a new cell
                        target.dispatchEvent(new Event('mouseenter'));
                    }
                };
                cell.ontouchend = () => stopDrawing();

                board.appendChild(cell);
            }
        }
    }

    function stopDrawing() { isDrawing = false; }

    function handlePathInput(r, c) {
        if(pSolved || pGrid[r][c] === 'W' || pGrid[r][c] === 'S') return;

        // Timer Start
        if(!pActive) {
            pActive = true;
            pStart = Date.now();
            pTimer = setInterval(() => {
                let s = Math.floor((Date.now()-pStart)/1000);
                document.getElementById('p-timer').textContent = 
                    Math.floor(s/60).toString().padStart(2,'0')+":"+(s%60).toString().padStart(2,'0');
            }, 1000);
        }

        // Check if the cell is already in the path
        const existingIndex = pPath.findIndex(pos => pos.r === r && pos.c === c);

        if (existingIndex !== -1) {
            // REVERT LOGIC: If user moves back to a previous cell in the path, 
            // truncate the path to that point.
            pPath = pPath.slice(0, existingIndex + 1);
            renderPathBoard();
            return;
        }

        const last = pPath.length > 0 ? pPath[pPath.length-1] : {r: 0, c: 0};
        const dist = Math.abs(r - last.r) + Math.abs(c - last.c);

        // Only add if adjacent
        if(dist === 1) {
            pPath.push({r, c});
            if(pGrid[r][c] === 'E') {
                pSolved = true;
                isDrawing = false;
                const timeTaken = Math.floor((Date.now() - pStart) / 1000);
            StatsManager.saveGame('pathFinder', pSize, timeTaken);
                clearInterval(pTimer);
                document.getElementById('p-status').textContent = "Path verified. Route saved!";
            }
            renderPathBoard();
        }
    }
    
    // Manual Undo for the UI
    function undoPath() {
        if (pSolved || pPath.length === 0) return;
        pPath.pop();
        renderPathBoard();
    }

    // --- 2048 ENGINE ---
    let tGrid = [], tScore = 0, nextTileId = 0;
    let startX, startY;

    function init2048() {
        tGrid = Array(4).fill().map(() => Array(4).fill(null));
        tScore = 0; 
        nextTileId = 0;
        document.getElementById('t-tile-layer').innerHTML = '';
        addRandomTile(); 
        addRandomTile();
        render2048();
        setup2048Input();
    }

    function addRandomTile() {
        let empty = [];
        for(let r=0; r<4; r++) for(let c=0; c<4; c++) if(!tGrid[r][c]) empty.push({r, c});
        if(empty.length) {
            let {r, c} = empty[Math.floor(Math.random() * empty.length)];
            tGrid[r][c] = { val: Math.random() < 0.9 ? 2 : 4, id: nextTileId++, merged: false };
        }
    }

    function render2048() {
        const layer = document.getElementById('t-tile-layer');
        const activeIds = new Set();

        tGrid.forEach((row, r) => {
            row.forEach((tileData, c) => {
                if (tileData) {
                    activeIds.add(tileData.id.toString());
                    let tileElem = document.getElementById(`tile-${tileData.id}`);
                    
                    if (!tileElem) {
                        tileElem = document.createElement('div');
                        tileElem.id = `tile-${tileData.id}`;
                        layer.appendChild(tileElem);
                    }

                    tileElem.style.transform = `translate(${c * 80}px, ${r * 80}px)`;
                    tileElem.textContent = tileData.val;
                    tileElem.className = `t-tile t-${tileData.val} ${tileData.merged ? 't-merged' : ''}`;
                }
            });
        });

        Array.from(layer.children).forEach(child => {
            const id = child.id.replace('tile-', '');
            if (!activeIds.has(id)) {
                child.style.opacity = "0"; // Fade out
                setTimeout(() => child.remove(), 300); 
            }
        });

        document.getElementById('t-score').textContent = `Score: ${tScore}`;
    }

    function move2048(dir) {
        let moved = false;
        // Clockwise Rotation: [r][c] -> [c][reverse r]
        const rotateCW = (m) => m[0].map((_, i) => m.map(row => row[i]).reverse());
        let temp = JSON.parse(JSON.stringify(tGrid));
        
        // CORRECTED MAPPING:
        // Left: 0 | Down: 1 | Right: 2 | Up: 3
        let rotCount = 0;
        if (dir === 'D') rotCount = 1;
        if (dir === 'R') rotCount = 2;
        if (dir === 'U') rotCount = 3;

        // Rotate the grid to normalize the move to "Slide Left"
        for(let i=0; i<rotCount; i++) temp = rotateCW(temp);

        // CORE SLIDE & MERGE
        for(let r=0; r<4; r++) {
            let row = temp[r].filter(v => v !== null);
            for(let i=0; i<row.length-1; i++) {
                if(row[i].val === row[i+1].val) {
                    row[i].val *= 2;
                    row[i].merged = true; 
                    tScore += row[i].val;
                    row.splice(i+1, 1);
                    moved = true;
                }
            }
            while(row.length < 4) row.push(null);
            temp[r] = row;
        }

        // ROTATE BACK: Restoration requires (4 - rotCount) clockwise turns
        let reverseRotations = (4 - rotCount) % 4;
        for(let i=0; i<reverseRotations; i++) temp = rotateCW(temp);
        
        // Check if the state actually changed to avoid adding a tile on a 'wall-bump'
        const stateChanged = JSON.stringify(tGrid) !== JSON.stringify(temp);
        
        if (stateChanged) {
            tGrid = temp;
            addRandomTile();
            render2048();
            checkGameOver();
        }
    }

    function checkGameOver() {
        let canMove = false;
        for(let r=0; r<4; r++) {
            for(let c=0; c<4; c++) {
                if(!tGrid[r][c]) { canMove = true; break; }
                if(c<3 && tGrid[r][c].val === tGrid[r][c+1].val) { canMove = true; break; }
                if(r<3 && tGrid[r][c].val === tGrid[r+1][c].val) { canMove = true; break; }
            }
        }
        if(!canMove) {
            alert("Game Over! Score: " + tScore);
            StatsManager.saveGame('game2048', 4, 0, { score: tScore });
        }
    }

    function setup2048Input() {
        const board = document.getElementById('t-board-container');
        
        // Touch events
        board.addEventListener('touchstart', e => {
            startX = e.touches[0].pageX;
            startY = e.touches[0].pageY;
        }, {passive: true});

        board.addEventListener('touchend', e => {
            const dx = e.changedTouches[0].pageX - startX;
            const dy = e.changedTouches[0].pageY - startY;
            if (Math.abs(dx) > 30 || Math.abs(dy) > 30) {
                if (Math.abs(dx) > Math.abs(dy)) move2048(dx > 0 ? 'R' : 'L');
                else move2048(dy > 0 ? 'D' : 'U');
            }
        }, {passive: true});

        // Mouse events (Drag)
        board.onmousedown = (e) => { startX = e.pageX; startY = e.pageY; };
        board.onmouseup = (e) => {
            const dx = e.pageX - startX;
            const dy = e.pageY - startY;
            if (Math.max(Math.abs(dx), Math.abs(dy)) > 30) {
                if (Math.abs(dx) > Math.abs(dy)) move2048(dx > 0 ? 'R' : 'L');
                else move2048(dy > 0 ? 'D' : 'U');
            }
        };

        // Keyboard
        window.onkeydown = (e) => {
            if(["ArrowUp","ArrowDown","ArrowLeft","ArrowRight"].includes(e.key)) {
                e.preventDefault();
                move2048(e.key.replace("Arrow","")[0]);
            }
        };
    }

    // Initialize both
    startNonogram(5);
    startMines(8, 10);
    startBinary(4);
    startPath(8);
    setup2048Input();
    init2048();

    // At the bottom of your <script>
    window.addEventListener('load', () => {
        // Show stats for all games immediately on page load
        StatsManager.renderBadges('nonogram');
        StatsManager.renderBadges('minesweeper');
        StatsManager.renderBadges('binaryLogic');
        StatsManager.renderBadges('pathFinder');
    });
    // Global reference for refreshing
    window.updateChart = renderHistoryChart;

    function renderHighlights() {
        const highlightContainer = document.getElementById('funny-highlights');
        if (!highlightContainer) return;

        const items = StatsManager.getHighlights();
        
        // Ensure the grid is responsive
        highlightContainer.style.gridTemplateColumns = "repeat(auto-fit, minmax(110px, 1fr))";

        highlightContainer.innerHTML = items.map(item => `
            <div style="background: #322e2e; border: 1px solid #30363d; border-radius: 6px; padding: 12px; text-align: center; position: relative;">
                <span title="${item.info}" style="position: absolute; top: 5px; right: 8px; cursor: help; font-size: 0.9rem; color: #ffffff; opacity: 0.9;">ⓘ</span>
                <div style="font-size: 2.2rem; margin-bottom: 1px;">${item.icon}</div>
                <div style="color: #ffffff; font-size: 1rem; text-transform: uppercase; letter-spacing: 0.5px; font-weight: 600;">${item.title}</div>
                <div style="color: ${item.color || '#ffffff'}; font-weight: bold; font-size: 1.2rem;">${item.val}</div>
            </div>
        `).join('');
    }

    // --- BACK TO TOP LOGIC ---
    const topBtn = document.getElementById("back-to-top");

    // Show button when user scrolls down 300px from the top
    window.onscroll = function() {
        if (document.body.scrollTop > 300 || document.documentElement.scrollTop > 300) {
            topBtn.style.display = "block";
        } else {
            topBtn.style.display = "none";
        }
    };

    // Scroll to the top of the document
    function scrollToTop() {
        window.scrollTo({
            top: 0,
            behavior: 'smooth'
        });
    }
</script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>