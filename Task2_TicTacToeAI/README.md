<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Tic Tac Toe AI - CodSoft</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #121212;
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 40px;
  }

  h2 {
    color: #00bfa5;
  }

  .board {
    display: grid;
    grid-template-columns: repeat(3, 120px);
    grid-template-rows: repeat(3, 120px);
    gap: 10px;
    margin-top: 20px;
  }

  .cell {
    width: 120px;
    height: 120px;
    background: #1e1e1e;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 48px;
    cursor: pointer;
    border-radius: 12px;
    transition: 0.2s;
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  }

  .cell:hover {
    background: #004d40;
  }

  .controls {
    margin-top: 20px;
  }

  button {
    padding: 10px 18px;
    border: none;
    border-radius: 12px;
    background: #00bfa5;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
    transition: 0.3s;
    font-weight: bold;
  }

  button:hover {
    background: #00796b;
  }

  #status {
    margin-top: 20px;
    font-size: 18px;
  }
</style>
</head>
<body>
<h2>Tic Tac Toe AI - You: X | AI: O</h2>
<div class="board" id="board"></div>
<div id="status">Your move!</div>
<div class="controls">
  <button id="reset">Reset Game</button>
</div>

<script>
  const boardEl = document.getElementById('board');
  const statusEl = document.getElementById('status');
  const resetBtn = document.getElementById('reset');

  let board = Array(9).fill(null); // null, 'X', 'O'
  const human = 'X', ai = 'O';

  const wins = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];

  function render() {
    boardEl.innerHTML = '';
    for(let i=0; i<9; i++){
      const cell = document.createElement('div');
      cell.className = 'cell';
      cell.dataset.index = i;
      cell.textContent = board[i] || '';
      cell.addEventListener('click', onClickCell);
      boardEl.appendChild(cell);
    }
    const winner = checkWinner(board);
    statusEl.textContent = winner || "Your move!";
  }

  function onClickCell(e){
    const i = Number(e.currentTarget.dataset.index);
    if(board[i] || checkWinner(board)) return;
    board[i] = human;
    if(!checkWinner(board)){
      const mv = bestMove(board);
      if(mv !== null) board[mv] = ai;
    }
    render();
  }

  function resetGame(){
    board = Array(9).fill(null);
    render();
  }
  resetBtn.addEventListener('click', resetGame);

  function checkWinner(b){
    for(const [a,c,d] of wins){
      if(b[a] && b[a] === b[c] && b[a] === b[d]){
        return b[a] === human ? "You win!" : "AI wins!";
      }
    }
    if(!b.includes(null)) return "Draw!";
    return null;
  }

  // Minimax AI
  function bestMove(b){
    let bestScore = -Infinity, move = null;
    for(let i=0;i<9;i++){
      if(!b[i]){
        b[i] = ai;
        const score = minimax(b, 0, false);
        b[i] = null;
        if(score > bestScore){ bestScore = score; move = i; }
      }
    }
    return move;
  }

  function minimax(b, depth, isMaximizing){
    const winner = winnerScore(b);
    if(winner !== null) return winner;
    if(isMaximizing){
      let best = -Infinity;
      for(let i=0;i<9;i++){
        if(!b[i]){
          b[i] = ai;
          best = Math.max(best, minimax(b, depth+1, false));
          b[i] = null;
        }
      }
      return best;
    } else {
      let best = Infinity;
      for(let i=0;i<9;i++){
        if(!b[i]){
          b[i] = human;
          best = Math.min(best, minimax(b, depth+1, true));
          b[i] = null;
        }
      }
      return best;
    }
  }

  function winnerScore(b){
    for(const [a,c,d] of wins){
      if(b[a] && b[a] === b[c] && b[a] === b[d]){
        return b[a] === ai ? 10 : -10;
      }
    }
    if(!b.includes(null)) return 0;
    return null;
  }

  render();
</script>
</body>
</html>
