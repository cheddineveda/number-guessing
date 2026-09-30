const minNumber = 1;
const maxNumber = 100;
const maxAttempts = 10;

let secretNumber = 0;
let attemptsUsed = 0;
const guessedNumbers = [];

const guessForm = document.getElementById('guess-form');
const guessInput = document.getElementById('guess-input');
const messageEl = document.getElementById('message');
const attemptsEl = document.getElementById('attempts');
const historyEl = document.getElementById('history');
const restartButton = document.getElementById('restart-button');

function startNewGame() {
  secretNumber = Math.floor(Math.random() * (maxNumber - minNumber + 1)) + minNumber;
  attemptsUsed = 0;
  guessedNumbers.length = 0;

  guessInput.value = '';
  guessInput.disabled = false;
  guessInput.focus();

  messageEl.textContent = 'Start guessing!';
  messageEl.style.color = '#e5e7eb';
  attemptsEl.textContent = '0';
  historyEl.textContent = 'None yet';
}

function updateHistory() {
  historyEl.textContent = guessedNumbers.length ? guessedNumbers.join(', ') : 'None yet';
}

function setMessage(text, color = '#e5e7eb') {
  messageEl.textContent = text;
  messageEl.style.color = color;
}

function endGame(won) {
  guessInput.disabled = true;

  if (won) {
    setMessage(`Congratulations! You guessed the number ${secretNumber} in ${attemptsUsed} attempts.`, '#22c55e');
  } else {
    setMessage(`Game over! The number was ${secretNumber}.`, '#ef4444');
  }
}

guessForm.addEventListener('submit', (event) => {
  event.preventDefault();

  const guess = Number(guessInput.value);

  if (!Number.isInteger(guess) || guess < minNumber || guess > maxNumber) {
    setMessage('Please enter a whole number between 1 and 100.', '#fbbf24');
    guessInput.value = '';
    return;
  }

  attemptsUsed += 1;
  guessedNumbers.push(guess);
  attemptsEl.textContent = String(attemptsUsed);
  updateHistory();

  if (guess === secretNumber) {
    endGame(true);
    return;
  }

  if (attemptsUsed >= maxAttempts) {
    endGame(false);
    return;
  }

  if (guess < secretNumber) {
    setMessage('Too low! Try a higher number.', '#38bdf8');
  } else {
    setMessage('Too high! Try a lower number.', '#38bdf8');
  }

  guessInput.value = '';
  guessInput.focus();
});

restartButton.addEventListener('click', startNewGame);

startNewGame();
