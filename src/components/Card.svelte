<script>
    export let face;
    export let cardClick = () => {};
</script>

<div class="card" on:click={cardClick} role="button" tabindex="0">
    <div class="card-inner">
        <div class="card-inner-front">❓</div>
        <div class="card-inner-back">{face}</div>
    </div>
</div>