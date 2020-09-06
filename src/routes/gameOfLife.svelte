
<script>
	import GameBoard from '../components/GameBoard.svelte';
	import { onDestroy } from 'svelte';

    let canvasWidth = 120;
    let canvasHeight = 96;
    let isPlaying = false;
    const acron = [
        [72, 64],[73,64],[73,62],[75,63],[76,64],[77,64],[78,64]
			]
	let speed = 50;
	let updateInterval;
	
	const setUpdateInterval = (fn) => {
		updateInterval = setInterval(() => {
                fn();
			}, speed);
	}
	

	const togglePlay = (fn) => {
		if(isPlaying) {
			clearInterval(updateInterval);
		}
		else {
			setUpdateInterval(fn);
		}
		isPlaying = !isPlaying;
	}

	onDestroy(() => {
		clearInterval(updateInterval);
	})
	</script>

<style>
	.gol-container {
		margin: 0 0 1em 0;
		line-height: 1.5;
	}
</style>

<svelte:head>
	<title>GOF</title>
</svelte:head>

<div class="gol-container">
    <div id="game">
		<GameBoard 
			design={acron} 
			canvasHeight={canvasHeight}	
			canvasWidth={canvasWidth}
			toggle={togglePlay}
		/>
    </div>
</div>