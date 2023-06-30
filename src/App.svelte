<script>
    import Game from "./components/Game.svelte";
    import SvelteLogo from "./assets/SvelteLogo.svelte";

    import Fa from 'svelte-fa'
    import { faChevronDown } from '@fortawesome/free-solid-svg-icons'

    let playing = false;
    $: className = playing ? 'playing' : 'not-playing'
    $: {
      if (playing) {
        document.body.classList.remove('no-scroll');
      } else {
        document.body.classList.add('no-scroll');
      }
    }
    const toggleGameStatus = () => {
        playing = !playing
    };

    let chosenMatches = 6;

    const handleSelectChange = (e) => {
      chosenMatches = parseInt(e.target.value);
    }

</script>

<main class={className}>
    {#if !playing}
        <SvelteLogo />
        <h1>Memory Card Game</h1>
        <button class="play-button" on:click={toggleGameStatus}>Play</button>
        <small>Maximum matches:</small>
        <div class="dropdown">
          <select on:change={handleSelectChange}>
            <option value="2">2</option> <!-- for development -->
            <option value="6">6</option>
            <option value="9">9</option>
            <option value="13">13</option>
            <option value="16">16</option>
          </select>
          <Fa icon={faChevronDown} />
        </div>
        <p class="createdWith">Created with Svelte.js</p>
    {:else}
        <Game on:gameOver={toggleGameStatus} {chosenMatches} />
    {/if}
</main>