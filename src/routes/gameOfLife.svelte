<script context="module">
    import GameBoard from '../components/GameBoard.svelte';
	console.log("hello");
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
        <GameBoard />
    </div>
</div>