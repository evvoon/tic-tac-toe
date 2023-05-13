<script>
  import { onMount } from "svelte";

  let board = [null, null, null, null, null, null, null, null, null];
  let currentPlayer = "X";
  let gameOver = false;

  function handleClick(event) {
    const index = event.target.dataset.index;
    board[index] = currentPlayer;
    if (checkForWinner()) {
      gameOver = true;
    } else {
      currentPlayer = currentPlayer === "X" ? "O" : "X";
    }
  }

  function checkForWinner() {
    // Check rows
    for (let i = 0; i < 9; i += 3) {
      if (board[i] && board[i] === board[i + 1] && board[i] === board[i + 2]) {
        return board[i];
      }
    }

    // Check columns
    for (let i = 0; i < 3; i++) {
      if (board[i] && board[i] === board[i + 3] && board[i] === board[i + 6]) {
        return board[i];
      }
    }

    // Check diagonals
    if (board[0] && board[0] === board[4] && board[0] === board[8]) {
      return board[0];
    }
    if (board[2] && board[2] === board[4] && board[2] === board[6]) {
      return board[2];
    }

    // No winner
    return null;
  }

  function resetGame() {
    board = [null, null, null, null, null, null, null, null, null];
    currentPlayer = "X";
    gameOver = false;
  }

  onMount(resetGame);
</script>

<div class="board">
  {#each board as square, index}
    <div
      class="square"
      data-index={index}
      data-player={square}
      on:click={handleClick}
    >
      {square || " "}
    </div>
  {/each}
</div>

{#if gameOver}
  <p>
    {#if checkForWinner() === "X"}
      Player 1 wins!
    {:else if checkForWinner() === "O"}
      Player 2 wins!
    {:else}
      It's a draw!
    {/if}
  </p>
  <button on:click={resetGame}>Play again</button>
{/if}

<style>
  .board {
    display: flex;
    flex-wrap: wrap;
    width: 300px;
    height: 300px;
    border: 2px solid black;
  }

  .square {
    width: 100px;
    height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 48px;
    font-weight: bold;
    cursor: pointer;
  }

  .square:hover {
    background-color: rgb(87, 87, 87, 0.2);
  }

  .square[data-player="X"] {
    color: rgb(158 209 249);
  }

  .square[data-player="O"] {
    color: rgb(255 230 127);
  }
</style>
