<script>
    import Game from "./components/Game.svelte";
    import SvelteLogo from "./assets/SvelteLogo.svelte";
    import Modal from "./components/Modal.svelte";

    import Fa from 'svelte-fa'
    import { faChevronDown } from '@fortawesome/free-solid-svg-icons'

    let playing = false;
    let showScoreBoard = false;

    $: className = playing ? 'playing' : 'not-playing'
    $: classNameSecondary = showScoreBoard ? 'max-w-100' : '';

    $: {
      if (playing) document.body.classList.remove('no-scroll');
      else document.body.classList.add('no-scroll');
    }
    const toggleGameStatus = () => {
        playing = !playing
    };

    let possibleMatches = 6;
    const options = [6, 9, 13, 16];

    const handleSelectChange = (e) => {
      possibleMatches = parseInt(e.target.value);
    }

    const closeScoreBoard = (e) => {
      if (e.target !== e.currentTarget) return;
      if (e.target.querySelector('.card-element')) e.target.querySelector('.card-element').classList.add('fade-out')

      setTimeout(() => {
            showScoreBoard = !showScoreBoard
      }, 450);
    }

    let scores = []

    const openScoreBoard = () => {
      showScoreBoard = !showScoreBoard

      const storedData = localStorage.getItem('memorycardgame');

      if (storedData) {
        scores = JSON.parse(storedData);
      }
    }
</script>

<main class={`${className} ${classNameSecondary}`}>
  {#if !playing}
      <SvelteLogo />
      <h1>Memory Card Game</h1>
      <button class="menu-button mb-4" on:click={toggleGameStatus}>Play</button>
      <button class="menu-button" on:click={openScoreBoard}>Score</button>
      <small>Maximum matches:</small>
      <div class="dropdown">
        <select on:change={handleSelectChange}>
          {#each options as option}
            <option value={option} selected={option === possibleMatches}>{option}</option>
          {/each}
        </select>
        <Fa icon={faChevronDown} />
      </div>
      <p class="createdWith">Created with Svelte.js</p>

      {#if showScoreBoard}
        <Modal class="score-board" onClickOutside={closeScoreBoard}>
          <h2 class="title">Score History</h2>
          <div class="wrapper overflow-y-auto">
            <p class="pretitle">Difficulty</p>
            <p class="pretitle">Attempts</p>
            <p class="pretitle">Time</p>
            {#each scores as score}
              <p>{score.difficulty}</p>
              <p>{score.attempts}</p>
              <p>{score.time}</p>
            {/each}
          </div>
          <button class="btn" on:click={() => {
            scores = []
            localStorage.removeItem('memorycardgame')
          }}>Clear Score List</button>
        </Modal>
      {/if}

  {:else}
      <Game on:gameOver={toggleGameStatus} {possibleMatches} />
  {/if}
</main>