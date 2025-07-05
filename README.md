<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe Game</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Orbitron', monospace;
            background: #0a0a0a;
            color: #ffffff;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1rem;
            overflow-x: hidden;
        }

        .game-container {
            background: #0d1526;
            border-radius: 20px;
            padding: 2rem;
            box-shadow: 
                0 0 50px rgba(13, 21, 38, 0.8),
                inset 0 0 50px rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(0, 255, 255, 0.2);
            backdrop-filter: blur(10px);
            max-width: 500px;
            width: 100%;
            position: relative;
        }

        .game-container::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, 
                #1a2a4a, #0d1526, #2a3a5a, #1a2a4a);
            border-radius: 20px;
            z-index: -1;
            opacity: 0.6;
            animation: borderGlow 3s ease-in-out infinite alternate;
        }

        @keyframes borderGlow {
            0% { opacity: 0.3; }
            100% { opacity: 0.6; }
        }

        h1 {
            font-size: clamp(2rem, 5vw, 3rem);
            text-align: center;
            margin-bottom: 1.5rem;
            font-weight: 900;
            background: linear-gradient(45deg, #00ffff, #ff00ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 30px rgba(0, 255, 255, 0.5);
            animation: titlePulse 2s ease-in-out infinite alternate;
        }

        @keyframes titlePulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.02); }
        }

        .game-mode {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
        }

        .mode-btn {
            background: linear-gradient(45deg, #1a1a2e, #16213e);
            border: 2px solid #00ffff;
            color: #00ffff;
            padding: 0.75rem 1.5rem;
            border-radius: 25px;
            cursor: pointer;
            font-family: 'Orbitron', monospace;
            font-weight: 700;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            position: relative;
            overflow: hidden;
            min-width: 120px;
        }

        .mode-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 255, 255, 0.2), transparent);
            transition: left 0.5s ease;
        }

        .mode-btn:hover::before {
            left: 100%;
        }

        .mode-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0, 255, 255, 0.3);
        }

        .mode-btn.active {
            background: linear-gradient(45deg, #00ffff, #0099cc);
            color: #000;
            border-color: #00ffff;
            box-shadow: 0 0 20px rgba(0, 255, 255, 0.5);
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 1rem;
            margin: 2rem auto;
            max-width: 300px;
            width: 100%;
            background: rgba(0, 0, 0, 0.5);
            padding: 1.5rem;
            border-radius: 15px;
            border: 2px solid rgba(0, 255, 255, 0.3);
        }

        .cell {
            aspect-ratio: 1;
            background: linear-gradient(145deg, #1a1a2e, #16213e);
            border: 2px solid rgba(0, 255, 255, 0.3);
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: clamp(2rem, 6vw, 3rem);
            font-weight: 900;
            cursor: pointer;
            transition: all 0.3s ease;
            color: #fff;
            position: relative;
            overflow: hidden;
            min-height: 70px;
        }

        .cell::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(45deg, transparent, rgba(0, 255, 255, 0.1), transparent);
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .cell:hover::before {
            opacity: 1;
        }

        .cell:hover {
            transform: scale(1.05);
            border-color: #00ffff;
            box-shadow: 0 0 20px rgba(0, 255, 255, 0.4);
        }

        .cell.x {
            color: #ff0080;
            text-shadow: 0 0 20px rgba(255, 0, 128, 0.7);
            animation: cellGlow 0.5s ease-in-out;
        }

        .cell.o {
            color: #00ff80;
            text-shadow: 0 0 20px rgba(0, 255, 128, 0.7);
            animation: cellGlow 0.5s ease-in-out;
        }

        @keyframes cellGlow {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }

        .game-info {
            text-align: center;
            margin-top: 1.5rem;
        }

        .current-player {
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 1rem;
            padding: 1rem;
            background: linear-gradient(45deg, rgba(0, 255, 255, 0.1), rgba(255, 0, 255, 0.1));
            border-radius: 15px;
            border: 1px solid rgba(0, 255, 255, 0.3);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .reset-btn {
            background: linear-gradient(45deg, #ff0080, #8000ff);
            border: none;
            color: #fff;
            padding: 1rem 2rem;
            border-radius: 25px;
            cursor: pointer;
            font-family: 'Orbitron', monospace;
            font-weight: 700;
            font-size: 1rem;
            margin-top: 1rem;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            position: relative;
            overflow: hidden;
        }

        .reset-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.5s ease;
        }

        .reset-btn:hover::before {
            left: 100%;
        }

        .reset-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 30px rgba(255, 0, 128, 0.4);
        }

        .winner-message {
            font-size: 1.5rem;
            font-weight: 900;
            margin-top: 1rem;
            padding: 1.5rem;
            background: linear-gradient(45deg, #00ff80, #0080ff);
            color: #000;
            border-radius: 15px;
            animation: winnerPulse 1s ease-in-out infinite alternate;
            text-transform: uppercase;
            letter-spacing: 2px;
            box-shadow: 0 0 30px rgba(0, 255, 128, 0.5);
        }

        @keyframes winnerPulse {
            0% { transform: scale(1); box-shadow: 0 0 30px rgba(0, 255, 128, 0.5); }
            100% { transform: scale(1.02); box-shadow: 0 0 40px rgba(0, 255, 128, 0.8); }
        }

        .score-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 1rem;
            margin-top: 2rem;
            padding: 1.5rem;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            border: 1px solid rgba(0, 255, 255, 0.2);
        }

        .score-item {
            text-align: center;
            padding: 1rem;
            background: linear-gradient(145deg, #1a1a2e, #16213e);
            border-radius: 10px;
            border: 1px solid rgba(0, 255, 255, 0.2);
            transition: all 0.3s ease;
        }

        .score-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 255, 255, 0.2);
        }

        .score-label {
            font-size: 0.9rem;
            color: #00ffff;
            margin-bottom: 0.5rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .score-value {
            font-size: 2rem;
            font-weight: 900;
            color: #fff;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .game-container {
                padding: 1.5rem;
                margin: 1rem;
            }

            .game-board {
                gap: 0.75rem;
                padding: 1rem;
            }

            .mode-btn {
                padding: 0.6rem 1.2rem;
                font-size: 0.8rem;
                min-width: 100px;
            }

            .score-board {
                padding: 1rem;
                gap: 0.75rem;
            }

            .score-item {
                padding: 0.75rem;
            }

            .score-value {
                font-size: 1.5rem;
            }
        }

        @media (max-width: 480px) {
            .game-container {
                padding: 1rem;
            }

            .game-board {
                max-width: 250px;
                gap: 0.5rem;
                padding: 0.75rem;
            }

            .cell {
                min-height: 60px;
            }

            .current-player {
                font-size: 1rem;
                padding: 0.75rem;
            }

            .reset-btn {
                padding: 0.75rem 1.5rem;
                font-size: 0.9rem;
            }

            .winner-message {
                font-size: 1.2rem;
                padding: 1rem;
            }
        }

        /* Loading animation */
        .loading {
            opacity: 0;
            animation: fadeIn 0.5s ease-in-out forwards;
        }

        @keyframes fadeIn {
            to { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="game-container loading">
        <h1>⚡ TIC-TAC-TOE ⚡</h1>
        
        <div class="game-mode">
            <button class="mode-btn active" id="playerVsPlayer">🎮 PvP</button>
            <button class="mode-btn" id="playerVsComputer">🤖 vs AI</button>
        </div>

        <div class="game-board" id="gameBoard">
            <div class="cell" data-index="0"></div>
            <div class="cell" data-index="1"></div>
            <div class="cell" data-index="2"></div>
            <div class="cell" data-index="3"></div>
            <div class="cell" data-index="4"></div>
            <div class="cell" data-index="5"></div>
            <div class="cell" data-index="6"></div>
            <div class="cell" data-index="7"></div>
            <div class="cell" data-index="8"></div>
        </div>

        <div class="game-info">
            <div class="current-player" id="currentPlayer">🎯 Player X's Turn</div>
            <div id="gameStatus"></div>
            <button class="reset-btn" id="resetBtn">🔄 Reset Game</button>
        </div>

        <div class="score-board">
            <div class="score-item">
                <div class="score-label">Player X</div>
                <div class="score-value" id="scoreX">0</div>
            </div>
            <div class="score-item">
                <div class="score-label">Draws</div>
                <div class="score-value" id="scoreDraw">0</div>
            </div>
            <div class="score-item">
                <div class="score-label">Player O</div>
                <div class="score-value" id="scoreO">0</div>
            </div>
        </div>
    </div>

    <script>
        class TicTacToe {
            constructor() {
                this.board = Array(9).fill('');
                this.currentPlayer = 'X';
                this.gameActive = true;
                this.gameMode = 'pvp';
                this.scores = { X: 0, O: 0, draw: 0 };
                
                this.winningConditions = [
                    [0, 1, 2], [3, 4, 5], [6, 7, 8],
                    [0, 3, 6], [1, 4, 7], [2, 5, 8],
                    [0, 4, 8], [2, 4, 6]
                ];

                this.initializeGame();
            }

            initializeGame() {
                this.bindEvents();
                this.updateDisplay();
                this.loadScores();
            }

            bindEvents() {
                document.getElementById('gameBoard').addEventListener('click', (e) => {
                    if (e.target.classList.contains('cell')) {
                        const index = parseInt(e.target.dataset.index);
                        this.handleCellClick(index);
                    }
                });

                document.getElementById('playerVsPlayer').addEventListener('click', () => {
                    this.setGameMode('pvp');
                });

                document.getElementById('playerVsComputer').addEventListener('click', () => {
                    this.setGameMode('pvc');
                });

                document.getElementById('resetBtn').addEventListener('click', () => {
                    this.resetGame();
                });

                // Touch events for mobile
                document.addEventListener('touchstart', (e) => {
                    if (e.target.classList.contains('cell')) {
                        e.preventDefault();
                    }
                });
            }

            setGameMode(mode) {
                this.gameMode = mode;
                this.resetGame();
                
                document.getElementById('playerVsPlayer').classList.toggle('active', mode === 'pvp');
                document.getElementById('playerVsComputer').classList.toggle('active', mode === 'pvc');
            }

            handleCellClick(index) {
                if (!this.gameActive || this.board[index] !== '') {
                    return;
                }

                this.makeMove(index, this.currentPlayer);
                
                if (this.gameActive && this.gameMode === 'pvc' && this.currentPlayer === 'O') {
                    setTimeout(() => this.computerMove(), 700);
                }
            }

            makeMove(index, player) {
                this.board[index] = player;
                this.updateCell(index, player);
                
                if (this.checkWinner()) {
                    this.endGame(`🎉 Player ${player} Wins! 🎉`);
                    this.scores[player]++;
                    this.updateScores();
                    this.saveScores();
                } else if (this.board.every(cell => cell !== '')) {
                    this.endGame("🤝 It's a Draw! 🤝");
                    this.scores.draw++;
                    this.updateScores();
                    this.saveScores();
                } else {
                    this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
                    this.updateDisplay();
                }
            }

            computerMove() {
                if (!this.gameActive) return;

                const bestMove = this.findBestMove();
                if (bestMove !== -1) {
                    this.makeMove(bestMove, 'O');
                }
            }

            findBestMove() {
                // Check if AI can win
                for (let i = 0; i < 9; i++) {
                    if (this.board[i] === '') {
                        this.board[i] = 'O';
                        if (this.checkWinner()) {
                            this.board[i] = '';
                            return i;
                        }
                        this.board[i] = '';
                    }
                }

                // Check if AI needs to block player
                for (let i = 0; i < 9; i++) {
                    if (this.board[i] === '') {
                        this.board[i] = 'X';
                        if (this.checkWinner()) {
                            this.board[i] = '';
                            return i;
                        }
                        this.board[i] = '';
                    }
                }

                // Strategic moves
                if (this.board[4] === '') return 4; // Center

                const corners = [0, 2, 6, 8];
                const availableCorners = corners.filter(i => this.board[i] === '');
                if (availableCorners.length > 0) {
                    return availableCorners[Math.floor(Math.random() * availableCorners.length)];
                }

                const availableSpaces = this.board.map((cell, index) => cell === '' ? index : null)
                    .filter(i => i !== null);
                return availableSpaces.length > 0 ? 
                    availableSpaces[Math.floor(Math.random() * availableSpaces.length)] : -1;
            }

            checkWinner() {
                return this.winningConditions.some(condition => {
                    const [a, b, c] = condition;
                    return this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c];
                });
            }

            updateCell(index, player) {
                const cell = document.querySelector(`[data-index="${index}"]`);
                cell.textContent = player;
                cell.classList.add(player.toLowerCase());
            }

            updateDisplay() {
                const playerDisplay = document.getElementById('currentPlayer');
                if (this.gameMode === 'pvp') {
                    playerDisplay.textContent = `🎯 Player ${this.currentPlayer}'s Turn`;
                } else {
                    playerDisplay.textContent = this.currentPlayer === 'X' ? 
                        "🎯 Your Turn" : "🤖 AI Thinking...";
                }
            }

            updateScores() {
                document.getElementById('scoreX').textContent = this.scores.X;
                document.getElementById('scoreO').textContent = this.scores.O;
                document.getElementById('scoreDraw').textContent = this.scores.draw;
            }

            saveScores() {
                const scores = {
                    X: this.scores.X,
                    O: this.scores.O,
                    draw: this.scores.draw
                };
                // Store in memory for this session
                window.gameScores = scores;
            }

            loadScores() {
                if (window.gameScores) {
                    this.scores = { ...window.gameScores };
                    this.updateScores();
                }
            }

            endGame(message) {
                this.gameActive = false;
                const statusElement = document.getElementById('gameStatus');
                statusElement.innerHTML = `<div class="winner-message">${message}</div>`;
            }

            resetGame() {
                this.board = Array(9).fill('');
                this.currentPlayer = 'X';
                this.gameActive = true;
                
                document.querySelectorAll('.cell').forEach(cell => {
                    cell.textContent = '';
                    cell.classList.remove('x', 'o');
                });
                
                document.getElementById('gameStatus').innerHTML = '';
                this.updateDisplay();
            }
        }

        // Initialize game when page loads
        document.addEventListener('DOMContentLoaded', () => {
            new TicTacToe();
        });

        // Prevent zoom on double tap for mobile
        let lastTouchEnd = 0;
        document.addEventListener('touchend', (e) => {
            const now = (new Date()).getTime();
            if (now - lastTouchEnd <= 300) {
                e.preventDefault();
            }
            lastTouchEnd = now;
        }, false);
    </script>
</body>
</html>
