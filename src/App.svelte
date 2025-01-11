<!-- Svelte játékgyűjtemény -->

<script>
  import { onMount } from "svelte";

  let selectedGame = null;
  const games = ["Számkitaláló", "Klikkelj a szívre", "Színes dobozok", "Szöveg tükrözése", "Stopper"];
  let randomNumber, guess, message;
  let clicks = 0;
  let color = "white";
  let inputText = "";
  let time = 0, interval;

  // Számkitaláló init
  const initGuessGame = () => {
    randomNumber = Math.floor(Math.random() * 100) + 1;
    guess = "";
    message = "";
  };

  const checkGuess = () => {
    if (+guess === randomNumber) {
      message = "Talált!";
    } else if (+guess > randomNumber) {
      message = "Túl nagy!";
    } else {
      message = "Túl kicsi!";
    }
  };

  // Színes dobozok init
  const changeColor = () => {
    const colors = ["red", "blue", "green", "yellow", "purple"];
    color = colors[Math.floor(Math.random() * colors.length)];
  };

  // Stopper init
  const startTimer = () => {
    if (!interval) {
      interval = setInterval(() => {
        time++;
      }, 1000);
    }
  };

  const stopTimer = () => {
    clearInterval(interval);
    interval = null;
  };

  const resetTimer = () => {
    stopTimer();
    time = 0;
  };

  // Reset state when switching games
  const resetGame = () => {
    if (interval) stopTimer();
    clicks = 0;
    inputText = "";
    color = "white";
    initGuessGame();
  };

  onMount(() => initGuessGame());
</script>

<style>
  .container {
    font-family: Arial, sans-serif;
    text-align: center;
    padding: 20px;
    background: linear-gradient(120deg, #1dd41d, #08f514);
    min-height: 100vh;
  }
  .game-menu {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 20px;
  }
  .game-menu button {
    background-color: #050105;
    color: white;
    border: none;
    padding: 10px 20px;
    font-size: 1em;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s;
  }
  .game-menu button:hover {
    background-color: #49014a;
  }
  .game {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    max-width: 400px;
    margin: 0 auto;
  }
  .hidden {
    display: none;
  }
</style>

<div class="container">
  <h1>Játékgyűjtemény</h1>
  <div class="game-menu">
    {#each games as game, index}
      <button on:click={() => { selectedGame = index; resetGame(); }}>{game}</button>
    {/each}
  </div>

  <div class="game" class:hidden={selectedGame !== 0}>
    <!-- Számkitaláló -->
    <input type="number" bind:value={guess} placeholder="Adj meg egy számot!" />
    <button on:click={checkGuess}>Tippelés</button>
    <p>{message}</p>
  </div>

  <div class="game" class:hidden={selectedGame !== 1}>
    <!-- Klikkelj a szívre -->
    <button on:click={() => clicks++}>❤️ Klikkelj rám! ❤️</button>
    <p>Klikkek száma: {clicks}</p>
  </div>

  <div class="game" class:hidden={selectedGame !== 2}>
    <!-- Színes dobozok -->
    <div style="width: 100px; height: 100px; background-color: {color}; margin: 20px auto;" />
    <button on:click={changeColor}>Válts színt!</button>
  </div>

  <div class="game" class:hidden={selectedGame !== 3}>
    <!-- Szöveg tükrözése -->
    <input type="text" bind:value={inputText} placeholder="Írj valamit!" />
    <p>{inputText.split("").reverse().join("")}</p>
  </div>

  <div class="game" class:hidden={selectedGame !== 4}>
    <!-- Stopper -->
    <p>Idő: {time} másodperc</p>
    <button on:click={startTimer}>Indítás</button>
    <button on:click={stopTimer}>Megállítás</button>
    <button on:click={resetTimer}>Nullázás</button>
  </div>
</div>
