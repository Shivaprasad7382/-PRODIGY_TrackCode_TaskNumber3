# -PRODIGY_TrackCode_TaskNumber3
  Tic-Tac-Toe Web application
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Tic-Tac-Toe</title>
</head>

<body>
    <div class="game-container">
        <div class="board" id="board">
            <!-- Cells will be dynamically added here using JavaScript -->
        </div>
        <div class="status" id="status">Player X's turn</div>
        <button class="reset-button" onclick="resetGame()">Reset Game</button>
    </div>
    <script src="script.js"></script>
</body>

</html>
//css
body {
    font-family: 'Arial', sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
}

.game-container {
    text-align: center;
}

.board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    gap: 5px;
    margin: 20px 0;
}

.cell {
    width: 100px;
    height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    cursor: pointer;
    border: 1px solid #ccc;
}

.status {
    margin-bottom: 20px;
    font-size: 18px;
}

.reset-button {
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
}
//js
// Initial game state
let currentPlayer = 'X';
let gameBoard = ['', '', '', '', '', '', '', '', ''];
let gameActive = true;

// Function to handle cell click
function handleCellClick(index) {
    if (!gameActive || gameBoard[index] !== '') return;

    // Update the game board and check for a winner
    gameBoard[index] = currentPlayer;
    if (checkWinner()) {
        document.getElementById('status').innerText = `Player ${currentPlayer} wins!`;
        gameActive = false;
    } else if (gameBoard.every(cell => cell !== '')) {
        document.getElementById('status').innerText = 'It\'s a draw!';
        gameActive = false;
    } else {
        currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
        document.getElementById('status').innerText = `Player ${currentPlayer}'s turn`;
    }

    // Update the UI
    updateBoard();
}

// Function to update the game board in the UI
function updateBoard() {
    const boardElement = document.getElementById('board');
    boardElement.innerHTML = '';

    for (let i = 0; i < gameBoard.length; i++) {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.innerText = gameBoard[i];
        cell.addEventListener('click', () => handleCellClick(i));
        boardElement.appendChild(cell);
    }
}

// Function to reset the game
function resetGame() {
    currentPlayer = 'X';
    gameBoard = ['', '', '', '', '', '', '', '', ''];
    gameActive = true;
    document.getElementById('status').innerText = 'Player X\'s turn';
    updateBoard();
}

// Function to check for a winner
function checkWinner() {
    const winPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8], // Rows
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8], // Columns
        [0, 4, 8],
        [2, 4, 6] // Diagonals
    ];

    return winPatterns.some(pattern =>
        gameBoard[pattern[0]] !== '' &&
        gameBoard[pattern[0]] === gameBoard[pattern[1]] &&
        gameBoard[pattern[1]] === gameBoard[pattern[2]]
    );
}

// Initialize the board on page load
updateBoard();
