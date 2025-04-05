<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe</title>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            justify-content: center;
            margin-top: 20px;
        }
        .cell {
            width: 100px;
            height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            background: lightgray;
            border: 2px solid black;
            cursor: pointer;
        }
        .cell.disabled {
            pointer-events: none;
        }
        #status {
            margin-top: 20px;
            font-size: 1.5em;
        }
        button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 1em;
        }
    </style>
</head>
<body>
    <h1>Tic Tac Toe</h1>
    <div id="status">Player X's turn</div>
    <div class="board" id="board"></div>
    <button onclick="resetGame()">Restart</button>
    
  <script>
        const board = document.getElementById("board");
        const status = document.getElementById("status");
        let currentPlayer = "X";
        let gameBoard = ["", "", "", "", "", "", "", "", ""];
        let gameActive = true;
        
        function createBoard() {
            board.innerHTML = "";
            gameBoard.forEach((value, index) => {
                const cell = document.createElement("div");
                cell.classList.add("cell");
                cell.dataset.index = index;
                cell.textContent = value;
                cell.addEventListener("click", handleMove);
                board.appendChild(cell);
            });
        }
        
        function handleMove(event) {
            const index = event.target.dataset.index;
            if (gameBoard[index] || !gameActive) return;
            
            gameBoard[index] = currentPlayer;
            event.target.textContent = currentPlayer;
            
            if (checkWinner()) {
                status.textContent = `Player ${currentPlayer} wins!`;
                gameActive = false;
                return;
            }
            
            if (!gameBoard.includes("")) {
                status.textContent = "It's a draw!";
                gameActive = false;
                return;
            }
            
            currentPlayer = currentPlayer === "X" ? "O" : "X";
            status.textContent = `Player ${currentPlayer}'s turn`;
        }
        
        function checkWinner() {
            const winPatterns = [
                [0, 1, 2], [3, 4, 5], [6, 7, 8],
                [0, 3, 6], [1, 4, 7], [2, 5, 8],
                [0, 4, 8], [2, 4, 6]
            ];
            return winPatterns.some(pattern => {
                const [a, b, c] = pattern;
                return gameBoard[a] && gameBoard[a] === gameBoard[b] && gameBoard[a] === gameBoard[c];
            });
        }
        
        function resetGame() {
            gameBoard = ["", "", "", "", "", "", "", "", ""];
            gameActive = true;
            currentPlayer = "X";
            status.textContent = "Player X's turn";
            createBoard();
        }
        
        createBoard();
    </script>
</body>
</html>
