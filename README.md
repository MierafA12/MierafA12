### Hi there 👋 I am Mieraf 



// Handle theme changes
document.getElementById('theme').addEventListener('change', function() {
    const selectedTheme = this.value;
    const buttons = document.querySelectorAll('button');

    if (selectedTheme === 'classic') {
        buttons.forEach(btn => {
            btn.style.backgroundColor = 'white';
            btn.style.color = 'black';
            btn.style.fontFamily = 'Arial, sans-serif';
        });
    } else if (selectedTheme === 'fantasy') {
        buttons.forEach(btn => {
            btn.style.backgroundColor = '#FFD700';  // Gold background
            btn.style.color = '#8B0000';  // Dark red text
            btn.style.fontFamily = '"Comic Sans MS", cursive, sans-serif';
        });
    } else if (selectedTheme === 'modern') {
        buttons.forEach(btn => {
            btn.style.backgroundColor = '#333';  // Dark background
            btn.style.color = '#FFF';  // White text
            btn.style.fontFamily = 'Helvetica, sans-serif';
        });
    }
});

// const buttons = document.querySelector('button');	
// const body = document.querySelector('body');

// buttons.addEventListener('click', () => {
//     body.classList.toggle('active');
// }); 

// buttons.addEventListener('mouseenter', function() {
//     body.style.backgroundImage = 'url(o.jpg)';
    
// });

let currentPlayer = 'X'; // Start with 'X'
let board = ['', '', '', '', '', '', '', '', '']; // Represents the game state
let gameActive = false; // To track whether the game has started

const buttons = document.querySelectorAll('.all button');
const body = document.querySelector('body');
const winnerDisplay = document.getElementById('winner');
const startButton = document.getElementById('start');
const historyList = document.getElementById('history-list');
const resetButton = document.getElementById('reset');

// Function to handle cell clicks
function handleClick(event) {
    if (!gameActive) return; // Don't allow moves if the game hasn't started

    const button = event.target;
    const index = Array.from(buttons).indexOf(button);

    // If the cell is empty and no one has won
    if (board[index] === '' && !winnerDisplay.textContent) {
        button.textContent = currentPlayer; // Place the current player's mark
        board[index] = currentPlayer; // Update game state

        // Switch background based on current player
        if (currentPlayer === 'O') {
            body.classList.remove('player-x');
            body.classList.add('player-o');
        } else {
            body.classList.remove('player-o');
            body.classList.add('player-x');
        }

        checkWinner(); // Check if we have a winner
        currentPlayer = currentPlayer === 'X' ? 'O' : 'X'; // Switch player
    }
}

function updateHistory() {
    // Clear the current history
    historyList.innerHTML = '';

    // Append each move to the history list
    moveHistory.forEach(move => {
        const listItem = document.createElement('li');
        listItem.textContent = move;
        historyList.appendChild(listItem);
    });
}
// Function to check if a player has won
function checkWinner() {
    const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
        [0, 4, 8], [2, 4, 6]  // Diagonals
    ];

    for (const combination of winningCombinations) {
        const [a, b, c] = combination;
        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
            winnerDisplay.textContent = `Player ${currentPlayer} wins!`;
            gameActive = false; // Stop the game once we have a winner
            return;
        }
    }

    // If all cells are filled and no one has won, it's a draw
    if (board.every(cell => cell !== '')) {
        winnerDisplay.textContent = "It's a draw!";
        gameActive = false;
    }
}

// Start Game function
function startGame() {
    gameActive = true; // Game is active
    currentPlayer = 'X'; // X always starts first
    board = ['', '', '', '', '', '', '', '', '']; // Reset the game state
    buttons.forEach(button => button.textContent = ''); // Clear all buttons
    winnerDisplay.textContent = ''; // Clear the winner message
    body.classList.remove('player-o');
    body.classList.add('player-x'); // Default to X's background
}

// Reset Game function
function resetGame() {
    gameActive = false; // Stop the game
    currentPlayer = 'X'; // Reset to player X
    board = ['', '', '', '', '', '', '', '', '']; // Clear the board state
    buttons.forEach(button => button.textContent = ''); // Clear all button text
    winnerDisplay.textContent = ''; // Clear the winner message
    body.classList.remove('player-o');
    body.classList.add('player-x'); // Default to X's background
}

// Add event listeners to the buttons
buttons.forEach(button => button.addEventListener('click', handleClick));
startButton.addEventListener('click', startGame);
resetButton.addEventListener('click', resetGame);
