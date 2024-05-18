<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Monofett&display=swap" rel="stylesheet">

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Denk+One&display=swap" rel="stylesheet">


<script>
    let currentNumber = generateRandomNumber(10, 99); // Start with two-digit numbers
    let score = 0;
    let isGameOver = false;
    let backgroundImage = 'blue.gif'; // Default background image
  
    function handleKeyDown(event) {
      if (isGameOver) return;
      if (event.key !== 'ArrowUp' && event.key !== 'ArrowDown') {
      return; // Exit the function if the pressed key is not an arrow key
    }
      const isPrime = isPrimeNumber(currentNumber);
      const isCorrectKey =
        backgroundImage === 'red.gif'
          ? (isPrime && event.key === 'ArrowDown') || (!isPrime && event.key === 'ArrowUp')
          : (isPrime && event.key === 'ArrowUp') || (!isPrime && event.key === 'ArrowDown');
  
      if (isCorrectKey) {
        score++;
        const [min, max] = getNumberRange(score);
        currentNumber = generateRandomNumber(min, max);
        toggleBackgroundImage();
      } else {
        isGameOver = true;
        backgroundImage = 'red.gif'; // Set background image to 'red.gif' during game over
        updateBodyBackground();
      }
    }
  
    function generateRandomNumber(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }
  
    function isPrimeNumber(number) {
      if (number <= 1) return false;
      if (number <= 3) return true;
      if (number % 2 === 0 || number % 3 === 0) return false;
  
      for (let i = 5; i * i <= number; i += 6) {
        if (number % i === 0 || number % (i + 2) === 0) {
          return false;
        }
      }
  
      return true;
    }
  
    function getNumberRange(score) {
      let min, max;
      if (score < 20) {
        min = 10;
        max = 99;
      } else if (score < 30) {
        min = 100;
        max = 999;
      } else if (score < 50) {
        min = 1000;
        max = 9999;
      } else {
        min = 10000;
        max = 99999;
      }
      return [min, max];
    }
  
    function toggleBackgroundImage() {
      // 20% chance of toggling red background
      if (Math.random() < 0.2) {
        backgroundImage = 'red.gif';
      } else {
        backgroundImage = 'blue.gif';
      }
      updateBodyBackground();
    }
  
    function updateBodyBackground() {
      // Check if the 'document' object exists (client-side rendering)
      if (typeof document !== 'undefined') {
        document.body.style.backgroundImage = `url(${backgroundImage})`;
        document.body.style.backgroundSize = 'cover';
        document.body.style.backgroundRepeat = 'no-repeat';
        document.body.style.color = 'white';
      }
    }
  
    function resetGame() {
      currentNumber = generateRandomNumber(10, 99);
      score = 0;
      isGameOver = false;
      backgroundImage = 'blue.gif';
      updateBodyBackground();
    }
  
    // Set initial background image
    if (typeof document !== 'undefined') {
      updateBodyBackground();
    }
  </script>
  
  <main>
    
    {#if isGameOver}
        <h1>Prime</h1>
        <p class="game-over">It's over fr <br> </p> 
        <p class="end"> {score} is all you scored.  Cry me a river</p>
      <div class="play-again"> 
        <button class="play-again-btn" on:click={resetGame}>Play Again</button>
        </div>
    {:else}
        <div class="header-container">
            <h1>Prime</h1>
            <p class='score'>{score}</p>
        </div>  

        <p class='current-number'>{currentNumber}</p>
      <p class="rules">
        {#if backgroundImage === 'red.gif'}
            Press Up Arrow for composite numbers <br>
            Press Down Arrow for prime numbers <br>
            Opposite rules apply when the background turns red
        {:else}
            Press Up Arrow for composite numbers <br>
            Press Down Arrow for prime numbers <br>
            Opposite rules apply when the background turns red
        {/if}
      </p>
    {/if}
  </main>
  
  <svelte:window on:keydown={handleKeyDown} />
  
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Caesar+Dressing&family=Denk+One&display=swap');
    main {
      padding: 2% 20%;
      border-radius: 10px;
      align-items: center;
    }
    h1 {
        font-family: "Monofett", monospace;
        font-weight: 400;
        font-style: normal;
        font-size: 400%;
        opacity: 0.8;
    }

    .current-number {
        font-family: "Denk One", sans-serif;
        font-weight: 400;
        font-style: normal;
        text-align: center;
        font-size: 500%;
    }

    .header-container {
        display: grid;
    grid-template-columns: auto 1fr; /* 1fr takes up remaining space */
    align-items: center;
    gap: 1rem;
    justify-self: start; 
    }

    .score {
        font-family: "Caesar Dressing", system-ui;
        font-weight: 400;
        font-style: normal;
        font-size: 300%;
        margin: 0; /* Remove default margin */
        justify-self: end;
    }

    .rules {
        font-family: "Denk One", sans-serif;
        font-weight: 400;
        font-style: normal;
        text-align: center;
    }

    .game-over {
        font-family: "Caesar Dressing", system-ui;
        font-weight: 400;
        font-style: normal;
        font-size: 500%;
        text-align: center;
    }

    .end {
        font-family: "Denk One", sans-serif;
        font-weight: 400;
        font-style: normal;
        text-align: center;
        margin-top: -3.5rem;
    }

    .play-again {
        text-align: center;
    }

    .play-again-btn {
    background-color: black;
    color: red;
    border: none;
    padding: 0.8rem 1.2rem;
    font-family: "Caesar Dressing", system-ui;
    font-size: 1.2rem;
    border-radius: 0.4rem;
    box-shadow: 0 0.2rem 0.4rem rgba(0, 0, 0, 0.3);
    transition: all 0.3s ease;
    cursor: pointer;
    opacity: 75%;
    margin-top: 2rem;
  }

  .play-again-btn:hover {
    color: white;
    box-shadow: 0 0.4rem 0.8rem rgba(0, 0, 0, 0.5);
    transform: translateY(-0.2rem);
    opacity: 50%;
  }
  </style>