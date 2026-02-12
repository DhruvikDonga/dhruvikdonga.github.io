+++
title = "Games"
description = "Fun puzzle games by dhruvik"
date = "2023-12-25"
aliases = ["games", "puzzle", "fun"]
author = "Dhruvik Donga"
+++

> Logic is a muscle. These puzzles are the gym. Created for the moments between the code, to keep the mind sharp and the spirit curious.

{{< notice tip >}}
**Did you know?** Engaging in logical puzzles stimulates neuroplasticity—essentially "freshening" your 🧠 cells.

We have got the classics: [Nonogram](#nonogram), [Minesweeper](#minesweeper) and the brain-teaser [Idea Shuffle](#idea-shuffle).    

Do share with your friends and help them kill some time productively. 🚀
{{< /notice >}}

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

    /*Idea swapper*/
    #shuffle-wrapper.game-section { 
        width: 100%; 
        margin-bottom: 4rem; 
        display: flex;
        flex-direction: column;
        align-items: center; /* Center everything in the wrapper */
    }

    #shuffle-wrapper .s-level-text { 
        font-size: 2.2rem; /* Large, bold level indicator */
        font-weight: 800; 
        color: var(--accent-color); 
        margin-bottom: 10px;
        font-family: ui-monospace, SFMono-Regular, monospace;
    }

    #shuffle-wrapper .controls { 
        width: 100%;
        display: flex;
        justify-content: center; /* Center the Start Button */
        margin-bottom: 2rem; 
    }
    .s-container { 
        position: relative; 
        height: 180px; 
        width: 100%; 
        border-bottom: 2px solid var(--border-color); /* The Focus Line */
        display: flex; 
        justify-content: space-around; 
        align-items: flex-end; 
        padding-bottom: 10px; 
        margin-top: 20px;
    }
    .s-box { 
        width: 60px; 
        height: 60px; 
        background-color: var(--cell-bg); 
        border: 2px solid var(--border-color); 
        border-radius: 8px; 
        cursor: pointer; 
        display: flex; 
        justify-content: center; 
        align-items: center; 
        position: absolute; 
        transition: left 0.3s ease-in-out, transform 0.3s, background-color 0.3s; 
        bottom: 10px; 
    }
    .s-bulb { font-size: 30px; display: none; pointer-events: none; }
    .s-box.has-bulb .s-bulb { display: block; }
    .s-box.revealed { background-color: #161b22; transform: translateY(-30px); }
    .s-level-text { font-size: 1.1rem; font-weight: bold; color: var(--accent-color); }
    
</style>

## Nonogram
---
💡 [How to solve a nonogram](https://www.youtube.com/watch?v=zisu0Qf4TAI)  
💡 You can click the size options to generate a fresh game anytime.

<div class="game-section" id="nonogram-wrapper">
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
    <div id="n-status" style="color: var(--success-color); font-weight: bold; margin-top: 1rem;"></div>
</div>


## Minesweeper
---
💡 [How to solve a minesweeper](https://www.youtube.com/shorts/Zil7a1hetQE)  
💡 You can click the size options to generate a fresh game anytime.

<div class="game-section" id="mines-wrapper">
    <div class="controls">
        <button class="game-btn m-size-btn" id="m-btn-8" onclick="startMines(8, 10)">Easy (8x8)</button>
        <button class="game-btn m-size-btn" id="m-btn-12" onclick="startMines(12, 20)">Medium (12x12)</button>
    </div>
    <div class="timer-container">
        <div id="m-timer" class="timer">00:00</div>
        <div id="m-seed" class="seed-info"></div>
    </div>
    <div id="m-board" class="m-grid"></div>
    <div id="m-status" style="font-weight: bold; margin-top: 1rem;"></div>
</div>

## Idea Shuffle
---
💡 Click on levels to start next game

<div class="game-section shuffle-wrapper">
    <div class="s-level-text" id="s-level-display">LEVEL: 1/10</div>
    <div class="controls">
        <button class="game-btn" id="s-start-btn" style="padding: 10px 24px; font-size: 16px;" onclick="triggerShuffle()">Start Level</button>
    </div>
    <div class="s-container" id="s-box-area">
    </div>
    <div id="s-msg"></div>
</div>

<script>
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

        let p = 0; while(p < mines) {
            let r = Math.floor(rng.nextFloat()*size), c = Math.floor(rng.nextFloat()*size);
            if(mGrid[r][c] !== 'M') { mGrid[r][c] = 'M'; p++; }
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
    }

    function renderMBoard() {
        const board = document.getElementById('m-board');
        board.style.gridTemplateColumns = `repeat(${mSize}, 30px)`;
        board.innerHTML = '';
        for(let r=0; r<mSize; r++) for(let c=0; c<mSize; c++) {
            const cell = document.createElement('div'); cell.className = 'm-cell';
            if(mRev[r][c]) {
                cell.classList.add('revealed');
                if(mGrid[r][c]==='M') { cell.classList.add('mine'); cell.textContent = '💣'; }
                else if(mGrid[r][c]>0) { cell.textContent = mGrid[r][c]; cell.classList.add('num-'+mGrid[r][c]); }
            } else if(mFlag[r][c]) cell.classList.add('flagged');
            cell.onmousedown = (e) => {
                if(mOver) return;
                if(!mAct) { mAct = true; mStart = Date.now(); mTimer = setInterval(() => {
                    let s = Math.floor((Date.now()-mStart)/1000);
                    document.getElementById('m-timer').textContent = Math.floor(s/60).toString().padStart(2,'0')+":"+(s%60).toString().padStart(2,'0');
                }, 1000); }
                if(e.button===2) { mFlag[r][c] = !mFlag[r][c]; renderMBoard(); }
                else revealM(r, c);
            };
            cell.oncontextmenu = e => e.preventDefault();
            board.appendChild(cell);
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
            mOver = true; clearInterval(mTimer); document.getElementById('m-status').textContent = "VICTORY!";
            document.getElementById('m-status').style.color = "var(--success-color)";
        }
    }

    // --- IDEA SHUFFLE ---
    let s_lvl = 1, s_elements = [], s_bulb_idx = 0, s_shuffling = false;

    function triggerShuffle() {
        if (s_shuffling) return;
        const area = document.getElementById('s-box-area');
        const msg = document.getElementById('s-msg');
        const btn = document.getElementById('s-start-btn');
        
        msg.textContent = "Locating the idea...";
        msg.style.color = "var(--text-color)";
        btn.disabled = true;

        let count = 3 + Math.floor((s_lvl - 1) / 3); 
        let swaps = 4 + (s_lvl * 2);
        let ms = Math.max(150, 500 - (s_lvl * 35));

        area.innerHTML = '';
        s_elements = [];
        const rng = new SeededRNG(Date.now());
        s_bulb_idx = Math.floor(rng.nextFloat() * count);

        for (let i = 0; i < count; i++) {
            const b = document.createElement('div');
            b.className = 's-box';
            b.style.left = `${(100 / count) * i + (50 / count)}%`;
            b.style.transform = 'translateX(-50%)';
            if (i === s_bulb_idx) {
                b.classList.add('has-bulb');
                // The bulb starts hidden via CSS 'display: none' in the .s-bulb class
                b.innerHTML = '<span class="s-bulb" id="active-bulb">💡</span>';
            }
            area.appendChild(b);
            s_elements.push(b);
        }

        // 1. Reveal Phase
        setTimeout(() => {
            s_elements[s_bulb_idx].classList.add('revealed');
            const bulbElement = document.getElementById('active-bulb');
            bulbElement.style.display = 'block'; // Show bulb
            
            setTimeout(() => {
                s_elements[s_bulb_idx].classList.remove('revealed');
                bulbElement.style.display = 'none'; // HIDE BULB BEFORE SHUFFLE
                
                // 2. Shuffle Phase
                runShuffle(swaps, ms, rng);
            }, 1000);
        }, 400);
    }

    function runShuffle(total, speed, rng) {
        s_shuffling = true;
        let current = 0;
        const loop = setInterval(() => {
            if (current >= total) {
                clearInterval(loop);
                s_shuffling = false;
                document.getElementById('s-msg').textContent = "Where is the idea?";
                s_elements.forEach(box => {
                    box.onclick = () => validateBox(box);
                });
                return;
            }
            let i1 = Math.floor(rng.nextFloat() * s_elements.length);
            let i2 = Math.floor(rng.nextFloat() * s_elements.length);
            while(i1 === i2) i2 = Math.floor(rng.nextFloat() * s_elements.length);

            const p1 = s_elements[i1].style.left;
            const p2 = s_elements[i2].style.left;
            s_elements[i1].style.left = p2;
            s_elements[i2].style.left = p1;

            [s_elements[i1], s_elements[i2]] = [s_elements[i2], s_elements[i1]];
            current++;
        }, speed);
    }

    function validateBox(box) {
        if (s_shuffling) return;
        
        // Show the bulb if it's in this box
        const bulb = box.querySelector('.s-bulb');
        if (bulb) bulb.style.display = 'block';
        
        box.classList.add('revealed');
        const msg = document.getElementById('s-msg');
        
        if (box.classList.contains('has-bulb')) {
            msg.textContent = "State verified. Level Up!";
            msg.style.color = "var(--success-color)";
            setTimeout(() => {
                if(s_lvl < 10) s_lvl++;
                document.getElementById('s-level-display').textContent = `Level: ${s_lvl}/10`;
                cleanShuffle();
            }, 1200);
        } else {
            msg.textContent = "System mismatch. Resetting level...";
            msg.style.color = "#f85149";
            
            // Also reveal where the bulb actually was so the user knows
            const correctBox = s_elements.find(b => b.classList.contains('has-bulb'));
            if (correctBox) {
                correctBox.classList.add('revealed');
                correctBox.querySelector('.s-bulb').style.display = 'block';
            }
            
            setTimeout(cleanShuffle, 2000);
        }
    }

    function cleanShuffle() {
        document.getElementById('s-start-btn').disabled = false;
        document.getElementById('s-msg').textContent = "";
        document.getElementById('s-box-area').innerHTML = '';
    }
    // Initialize both
    startNonogram(5);
    startMines(8, 10);
    triggerShuffle();
</script>