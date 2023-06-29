<script>
    import Game from "./components/Game.svelte";
    import SvelteLogo from "./assets/SvelteLogo.svelte";

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
        <h2>Maximum matches:</h2>
        
        <select on:change={handleSelectChange}>
          <option value="6">6</option>
          <option value="9">9</option>
          <option value="13">13</option>
          <option value="16">16</option>
        </select>

        <button class="play-button" on:click={toggleGameStatus}>Play</button>
    {:else}
        <Game on:gameOver={toggleGameStatus} {chosenMatches} />
    {/if}
</main>