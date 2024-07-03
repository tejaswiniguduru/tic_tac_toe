const cells = document.querySelectorAll('[data-cell]');
const message = document.getElementById('message');
const restartButton = document.getElementById('restart-button');
const board = document.getElementById('board');
let currentPlayer = 'X';
let gameBoard = ['', '', '', '', '', '', '', '', ''];
let gameActive = true;

function checkWinner() {
    const winPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
    ];

    for (let pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (gameBoard[a] && gameBoard[a] === gameBoard[b] && gameBoard[a] === gameBoard[c]) {
            gameActive = false;
            highlightWinner(pattern);
            message.innerText = `${currentPlayer} wins!`;
        }
    }

    if (!gameBoard.includes('') && gameActive) {
        gameActive = false;
        message.innerText = "It's a draw!";
    }
}

function highlightWinner(pattern) {
    for (let i of pattern) {
        cells[i].classList.add('winner');
    }
}

function handleCellClick(e) {
    const cell = e.target;
    const index = [...cells].indexOf(cell);

    if (gameBoard[index] || !gameActive) return;

    gameBoard[index] = currentPlayer;
    cell.innerText = currentPlayer;
    cell.classList.add(currentPlayer);
    checkWinner();

    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
}

function restartGame() {
    gameActive = true;
    currentPlayer = 'X';
    gameBoard = ['', '', '', '', '', '', '', '', ''];
    cells.forEach(cell => {
        cell.innerText = '';
        cell.classList.remove('X', 'O', 'winner');
    });
    message.innerText = '';
}

cells.forEach(cell => {
    cell.addEventListener('click', handleCellClick);
});

restartButton.addEventListener('click', restartGame);