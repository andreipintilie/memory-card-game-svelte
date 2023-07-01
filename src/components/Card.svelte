<script>
  import { createEventDispatcher } from "svelte";

  let isLoading = true;
  $: loadingClass = isLoading ? 'loading' : '';

  let isRotating = false;
  $: rotatingClas = isRotating ? 'rotate' : '';

  export let face;
  export let order;
  export let cardClick = () => {};

  let dispatch = createEventDispatcher();

  function handleAnimationEnd() {
    this.style.opacity='1'

    if (!isLoading) return;

    setTimeout(() => {
      isRotating = true;
    }, 500)
    setTimeout(() => {
      isRotating = false;
    }, 2000)
    setTimeout(() => {
      isLoading = false;
      dispatch('onGameStart');
    }, 2500)
  }
</script>

<div
  class={`card ${loadingClass} ${rotatingClas}`}
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