<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #282c34;
            color: white;
            padding: 20px;
        }

        .game-container {
            display: inline-block;
            background-color: #444;
            padding: 20px;
            border-radius: 10px;
        }

        h1 {
            margin-bottom: 20px;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            margin-bottom: 20px;
        }

        .game-board div {
            width: 100px;
            height: 100px;
            background-color: #ddd;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            cursor: pointer;
            border-radius: 5px;
        }

        button {
            padding: 10px 20px;
            margin-top: 10px;
            font-size: 1em;
            cursor: pointer;
        }

        input {
            padding: 10px;
            font-size: 1em;
            margin-top: 10px;
        }

        #gameIdDisplay {
            margin-top: 20px;
            font-size: 1.2em;
            color: #f1c40f;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Tic Tac Toe</h1>
        <div class="game-board" id="game-board">
            <!-- 3x3 grid of cells -->
        </div>
        <div class="controls">
            <button onclick="createGame()">Create Game</button>
            <input type="text" id="joinGameIdInput" placeholder="Enter Game ID">
            <button onclick="joinGame()">Join Game</button>
            <p id="gameIdDisplay"></p> <!-- Show generated game ID here -->
        </div>
    </div>

    <script>
        // Variables for the game state
        let currentPlayer = 'X';
        let gameBoard = ['','','','','','','','',''];
        let gameId = ''; // Will store the generated game ID

        // Function to create a random 6-character Game ID
        function generateGameId() {
            return Math.random().toString(36).substring(2, 8);
        }

        // Function to create a new game
        function createGame() {
            gameId = generateGameId();
            alert(`Game created! Share this game ID: ${gameId}`);
            localStorage.setItem('gameId', gameId);
            document.getElementById('gameIdDisplay').innerText = `Game ID: ${gameId}`;
            resetGame();
        }

        // Function to join a game by entering the Game ID
        function joinGame() {
            const enteredGameId = document.getElementById('joinGameIdInput').value;
            if (enteredGameId === gameId) {
                alert(`Joining game with ID: ${gameId}`);
                loadGame();
            } else {
                alert('Invalid Game ID. Please try again.');
            }
        }

        // Function to reset the game board
        function resetGame() {
            gameBoard = ['','','','','','','','',''];
            currentPlayer = 'X';
            renderBoard();
        }

        // Function to render the game board
        function renderBoard() {
            const board = document.getElementById('game-board');
            board.innerHTML = '';
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.innerText = gameBoard[i];
                cell.addEventListener('click', () => handleClick(i));
                board.appendChild(cell);
            }
        }

        // Handle click event for each cell
        function handleClick(index) {
            if (gameBoard[index] === '') {
                gameBoard[index] = currentPlayer;
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                renderBoard();
                checkWinner();
            }
        }

        // Function to check if there's a winner
        function checkWinner() {
            const winPatterns = [
                [0, 1, 2], [3, 4, 5], [6, 7, 8], // Horizontal
                [0, 3, 6], [1, 4, 7], [2, 5, 8], // Vertical
                [0, 4, 8], [2, 4, 6]             // Diagonal
            ];

            for (let pattern of winPatterns) {
                const [a, b, c] = pattern;
                if (gameBoard[a] && gameBoard[a] === gameBoard[b] && gameBoard[a] === gameBoard[c]) {
                    alert(`${gameBoard[a]} wins!`);
                    resetGame();
                    return;
                }
            }

            if (!gameBoard.includes('')) {
                alert('It\'s a tie!');
                resetGame();
            }
        }

        // Dummy loadGame function (optional for expansion)
        function loadGame() {
            renderBoard();
        }

        // Initialize the game board
        renderBoard();
    </script>
</body>
</html>
