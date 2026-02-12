+++
title = "Games"
description = "Fun puzzle games by dhruvik"
date = "2023-12-25"
aliases = ["games", "puzzle", "fun"]
author = "Dhruvik Donga"
+++

## Nonogram
---
<style>
    :root {
        /* --bg-color: #0d1117; */
        --text-color: #c9d1d9;
        /* --border-color: #30363d; */
        --cell-bg: #2f4f4f;
        --cell-filled: #1f6feb;
        --cell-crossed: #8b949e;
        --hover-bg: #21262d;
        --accent-color: #58a6ff;
        --success-color: #238636;
        --btn-bg: #21262d;
        --btn-border: #363b42;
    }
    .nonogram-wrapper {
        width: 100%;
        color: var(--text-color);
        background-color: var(--bg-color);
        padding: 20px 0;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
    }
    .puzzle-container {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        width: 100%;
    }
    .controls, .tools, .timer-container {
        margin-bottom: 1.5rem;
        display: flex;
        gap: 8px;
        flex-wrap: wrap;
    }
    .game-btn {
        background-color: var(--btn-bg);
        color: var(--text-color);
        border: 1px solid var(--btn-border);
        padding: 5px 16px;
        cursor: pointer;
        border-radius: 6px;
        font-size: 14px;
        font-weight: 500;
        transition: 0.2s;
    }
    .game-btn:hover { background-color: #30363d; border-color: #8b949e; }
    .game-btn.active { background-color: #1f6feb; color: #ffffff; border-color: rgba(27, 31, 36, 0.15); }
    .game-container {
        display: grid;
        gap: 2px;
        background-color: var(--border-color);
        border: 1px solid var(--border-color);
        padding: 2px;
        border-radius: 4px;
        width: max-content;
    }
    .header-cell {
        background-color: var(--bg-color);
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: center;
        padding: 4px;
        font-size: 12px;
        font-family: ui-monospace, SFMono-Regular, SF Mono, Menlo, Consolas, monospace;
        color: #8b949e;
    }
    .header-cell.satisfied { color: var(--success-color); }
    .row-header { flex-direction: row; justify-content: flex-end; padding-right: 8px; }
    .cell {
        background-color: var(--cell-bg);
        cursor: pointer;
        width: 30px;
        height: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
        user-select: none;
    }
    .cell:hover { background-color: var(--hover-bg); }
    .cell.filled { background-color: var(--cell-filled); }
    .cell.crossed::after { content: "×"; color: var(--cell-crossed); font-size: 1.5rem; }
    .timer { font-size: 2rem; font-family: ui-monospace, SFMono-Regular, monospace; }
    .seed-info { font-size: 12px; color: #8b949e; margin-top: 5px; }
</style>

<div class="nonogram-wrapper">
    <div class="puzzle-container">
        <div class="controls">
            <button class="game-btn" id="btn-5" onclick="startGame(5)">5x5</button>
            <button class="game-btn" id="btn-8" onclick="startGame(8)">8x8</button>
            <button class="game-btn" id="btn-10" onclick="startGame(10)">10x10</button>
        </div>
        <div class="tools">
            <button class="game-btn active" id="tool-fill" onclick="setTool(0)">Fill ■</button>
            <button class="game-btn" id="tool-cross" onclick="setTool(2)">Cross ×</button>
        </div>
        <div class="timer-container">
            <div id="timer" class="timer">00:00</div>
            <div id="seed-display" class="seed-info"></div>
        </div>
        <div id="game-board" class="game-container"></div>
        <div id="status-message" style="color: var(--success-color); font-weight: bold; margin-top: 1.5rem; min-height: 1.5em;"></div>
    </div>
</div>

<script>
    class SeededRNG {
        constructor(seed) {
            this.m = 0x80000000;
            this.a = 1103515245;
            this.c = 12345;
            this.state = seed ? seed : Math.floor(Math.random() * (this.m - 1));
        }
        nextInt() { this.state = (this.a * this.state + this.c) % this.m; return this.state; }
        nextFloat() { return this.nextInt() / (this.m - 1); }
    }
    let currentSize = 5, solution = [], playerGrid = [], currentSeed = 0, timerInterval, startTime, currentTool = 0;
    function formatTime(seconds) {
        const m = Math.floor(seconds / 60).toString().padStart(2, '0');
        const s = (seconds % 60).toString().padStart(2, '0');
        return m + ":" + s;
    }
    function startTimer() {
        clearInterval(timerInterval);
        startTime = Date.now();
        timerInterval = setInterval(() => {
            const elapsed = Math.floor((Date.now() - startTime) / 1000);
            document.getElementById('timer').textContent = formatTime(elapsed);
        }, 1000);
    }
    function stopTimer() { clearInterval(timerInterval); }
    function setTool(tool) {
        currentTool = tool;
        document.getElementById('tool-fill').classList.toggle('active', tool === 0);
        document.getElementById('tool-cross').classList.toggle('active', tool === 2);
    }
    function calculateHints(line) {
        let hints = [], count = 0;
        for (let cell of line) {
            if (cell) count++;
            else if (count > 0) { hints.push(count); count = 0; }
        }
        if (count > 0) hints.push(count);
        if (hints.length === 0) hints.push(0);
        return hints;
    }
    function startGame(size) {
        currentSize = size;
        setTool(0);
        currentSeed = Date.now();
        const rng = new SeededRNG(currentSeed);
        document.querySelectorAll('.controls .game-btn').forEach(btn => btn.classList.remove('active'));
        document.getElementById('btn-' + size).classList.add('active');
        document.getElementById('seed-display').textContent = "Seed: " + currentSeed + " (" + size + "x" + size + ")";
        document.getElementById('status-message').textContent = '';
        document.getElementById('timer').textContent = "00:00";
        startTimer();
        solution = []; playerGrid = [];
        for (let r = 0; r < size; r++) {
            let row = [], pRow = [];
            for (let c = 0; c < size; c++) {
                row.push(rng.nextFloat() > 0.5);
                pRow.push(0);
            }
            solution.push(row);
            playerGrid.push(pRow);
        }
        if (!solution.some(row => row.some(cell => cell))) { solution[0][0] = true; }
        renderBoard();
    }
    function renderBoard() {
        const container = document.getElementById('game-board');
        container.innerHTML = '';
        container.style.gridTemplateColumns = "auto repeat(" + currentSize + ", 1fr)";
        const corner = document.createElement('div');
        corner.className = 'header-cell';
        container.appendChild(corner);
        for (let c = 0; c < currentSize; c++) {
            const hints = calculateHints(solution.map(row => row[c]));
            const playerCol = playerGrid.map(row => row[c] === 1);
            const isSatisfied = JSON.stringify(hints) === JSON.stringify(calculateHints(playerCol));
            const header = document.createElement('div');
            header.className = 'header-cell' + (isSatisfied ? ' satisfied' : '');
            header.innerHTML = hints.join('<br>');
            container.appendChild(header);
        }
        for (let r = 0; r < currentSize; r++) {
            const hints = calculateHints(solution[r]);
            const playerRow = playerGrid[r].map(cell => cell === 1);
            const isSatisfied = JSON.stringify(hints) === JSON.stringify(calculateHints(playerRow));
            const header = document.createElement('div');
            header.className = 'header-cell row-header' + (isSatisfied ? ' satisfied' : '');
            header.textContent = hints.join(' ');
            container.appendChild(header);
            for (let c = 0; c < currentSize; c++) {
                const cell = document.createElement('div');
                cell.className = 'cell';
                if (playerGrid[r][c] === 1) cell.classList.add('filled');
                if (playerGrid[r][c] === 2) cell.classList.add('crossed');
                cell.onmousedown = (e) => {
                    if (e.button === 2) handleInput(r, c, 2);
                    else handleInput(r, c, currentTool);
                };
                cell.oncontextmenu = (e) => e.preventDefault();
                container.appendChild(cell);
            }
        }
    }
    function handleInput(r, c, tool) {
        if (document.getElementById('status-message').textContent) return;
        const current = playerGrid[r][c];
        const target = (tool === 0) ? 1 : 2;
        playerGrid[r][c] = (current === target) ? 0 : target;
        renderBoard();
        checkWin();
    }
    function checkWin() {
        let win = true;
        for (let r = 0; r < currentSize; r++) {
            for (let c = 0; c < currentSize; c++) {
                if (solution[r][c] !== (playerGrid[r][c] === 1)) win = false;
            }
        }
        if (win) {
            document.getElementById('status-message').textContent = 'Puzzle Solved!';
            stopTimer();
        }
    }
    startGame(5);
</script>