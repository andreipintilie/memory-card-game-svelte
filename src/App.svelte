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
</script>

<main class={className}>
    {#if !playing}
        <SvelteLogo />
        <h1>Memory Card Game</h1>
        <button class="play-button" on:click={toggleGameStatus}>Play</button>
    {:else}
        <Game on:gameOver={toggleGameStatus} />
    {/if}
</main>