<script>
	
	import GameBoard from "./GameBoard.svelte";

	let gameRows = [];
	let isPlaying = false;
	let score = 0;
	let timeLeft = 0;
	let gameInterval = null;

	function startGame() {
		score = 0;
		isPlaying = true;
		startTimer();
		setUpRows();
	}

	function startTimer(){
		timeLeft = 5;
		gameInterval = setInterval(() => {
			if(timeLeft > 1){
				timeLeft = timeLeft - 1;				
			}else {
				gameOver();
			}
		}, 1000);
	};

	function gameOver() {
		isPlaying = false;
		stopTimer();
	};

	function circleClicked(isTarget) {
		
		if (isTarget.detail) {
			score = score + 1;
			resetTimer();
			setUpRows();
		} else {
			timeLeft = timeLeft - 1;
			if(timeLeft <= 0){
				gameOver();
			}
		}
  	};

	function resetTimer() {
		stopTimer();
		startTimer();
	};

	function stopTimer(){
		clearInterval(gameInterval);
		gameInterval = null;
	};

	function setUpRows() {
		let baseColor = Math.floor(Math.random() * 256);
		let restColors = Math.floor(Math.random() * 96) + 80;
		let normalRGB = [restColors, restColors, restColors];
		let offset = Math.floor(Math.random() * 2) === 0 ? -20 : 20;
		let randRGB = Math.floor(Math.random() * 3);
		let normalColor = '';
		let targetColor = '';

		normalRGB[randRGB] = baseColor;
		normalColor = `rgb(${normalRGB[0]}, ${normalRGB[1]}, ${normalRGB[2]})`
		let targetRGB = normalRGB.map((color, index) => {
			return index === randRGB ? color : color + offset;
		})

		targetColor = `rgb(${targetRGB[0]}, ${targetRGB[1]}, ${targetRGB[2]})`;

		let rows = [];
		let newTargetRow = Math.floor(Math.random() * 3);
		let newTargetColumn = Math.floor(Math.random() * 3);
		
		for (let i = 0; i < 3; i++) {
			let row = [];
		for (let j = 0; j < 3; j++) {
			let color = normalColor;
			let isTarget = false;
			if (i === newTargetRow && j === newTargetColumn) {
				color = targetColor;
				isTarget = true;
			}
			row.push(
				{
					key: `${i}, ${j}`,
					color: color,
					isTarget: isTarget
				}
			);
		}
		rows.push(row);
		}
		gameRows = rows;
	};

</script>


<style>
	@import url('https://fonts.googleapis.com/css2?family=Pangolin&display=swap');

	* {
		box-sizing: border-box;
		font-family: 'Pangolin', cursive;
		margin: 0;
		padding: 0;
	}

	main {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
	}

	.count-down {
		font-size: 3rem;
	}

	.score{
		font-size: 60px;
		margin: 0;
	}

	.button {
		outline: none;
		border: 3px solid black;
		background-color: white;
		font-size: 30px;
		border-radius: 20px;
		padding: 20px 40px;
		cursor: pointer;
	}
	
</style>


<main>
	{#if isPlaying}
		<h1 class='count-down'>{timeLeft}</h1>
		<GameBoard 
		gameRows={gameRows}
		on:clicked={circleClicked}/>
	{/if}
	<h1 class='score'>Score: {score}</h1>
	{#if !isPlaying}
		<button class='button' on:click={startGame}>
        	Start Game
        </button>
	{/if}
</main>


