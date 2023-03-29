<script>
    import TicTacToeGame from '$lib/components/TicTacToeGame.svelte'
    import GameSelector from "$lib/components/GameSelector.svelte";
    import GameArea from "$lib/components/GameArea.svelte";

    const AVAILABLE_GAMES = [
        { component: TicTacToeGame, displayName: 'Tic-Tac-Toe' }
    ];

    let currentGame;
</script>

<GameSelector allGames={AVAILABLE_GAMES} bind:selectedGame={currentGame}/>

<GameArea game={currentGame}/>
