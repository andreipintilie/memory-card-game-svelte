<script>
  export let face;
  export let order;
  export let cardClick = () => {};

  function handleAnimationEnd() {
    this.classList.remove('loading');
    this.style.opacity='1'
  }
</script>

<div
  class="card loading"
  on:click={cardClick}
  role="button"
  tabindex="0"
  style={`--order: ${order}`}
  on:animationend={handleAnimationEnd}
>
  <div class="card-inner">
    <div class="card-inner-front">❓</div>
    <div class="card-inner-back">{face}</div>
  </div>
</div>