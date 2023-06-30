<script>
    export let timer;
    export let matches;
    export let totalTries;
    export let totalMatches;

    let formattedTime = '';

    $: {
        const minutes = Math.floor(timer / 60).toString().padStart(2, '0');
        const seconds = (timer % 60).toString().padStart(2, '0');
        formattedTime = `${minutes}:${seconds}`;
    }
</script>

<p class="timer">{formattedTime}</p>
<h1>
    Matches: {matches}/{totalMatches} 
    <small>Tries: {totalTries}</small>
</h1>