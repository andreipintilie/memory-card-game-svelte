<div class="card-wrapper" on:click={onClickOutside} on:keydown={() => {}}>
    <div class={`card-element ${className}`}>
        <slot></slot>
    </div>
</div>

<script>
    let className = "";
    export { className as class };
    export let onClickOutside = () => {};
</script>