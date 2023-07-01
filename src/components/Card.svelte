<script>
  import { createEventDispatcher } from "svelte";

  let animationEnd = false;

  export let face;
  export let order;
  export let cardClick = () => {};

  let dispatch = createEventDispatcher();

  function handleAnimationEnd() {
    this.classList.remove('loading');
    this.style.opacity='1'

    if (animationEnd) return;

    setTimeout(() => {
      this.classList.add('rotate');
    }, 500)
    setTimeout(() => {
      this.classList.remove('rotate');
    }, 2000)
    setTimeout(() => {
      animationEnd = true;
      dispatch('onGameStart');
    }, 2500)
  }
</script>

<div
  class="card loading"
  on:click={cardClick}
  role="button"
  tabindex="0"
  style={`--order: ${order}`}
  on:animationend={handleAnimationEnd}
  on:keydown={() => {}}
>
  <div class="card-inner">
    <div class="card-inner-front">?</div>
    <div class="card-inner-back">{face}</div>
  </div>
</div>