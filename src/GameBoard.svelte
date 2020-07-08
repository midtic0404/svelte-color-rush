<script>
    import Circle from "./Circle.svelte";
    export let gameRows;
</script>

<style>
    .board {
		width: 400px;
		margin-bottom: 2rem;
	}

    .row {
		display: flex;
		width: 100%;
		justify-content: space-evenly;
		margin-top: 1rem;
		margin-bottom: 1rem;
	}
</style>

<div class="board">
    {#each gameRows as circleRow, index (index)}
        <div class="row">
            {#each circleRow as circleInfo (circleInfo.key)}
                <Circle 
                color={circleInfo.color} 
                isTarget={circleInfo.isTarget}
                on:clicked/>
            {/each}
        </div>
    {/each}
</div>