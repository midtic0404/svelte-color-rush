<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let color;
    export let isTarget;

    function handleClick() {
        dispatch('clicked', isTarget);
    }
    
</script>

<style>
    .circle {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        border: black solid 3px;
        cursor: pointer;
        transition: 0.1s ease;
    }
</style>

<div
      class='circle'
      style="background-color: {color}"
      on:click={handleClick}
    ></div>

