<script>
    import TicTacToeGame from '$lib/components/games/tictactoe/TicTacToeGame.svelte'
    import GameSelector from "$lib/components/ui/GameSelector.svelte";
    import GameArea from "$lib/components/ui/GameArea.svelte";

    const AVAILABLE_GAMES = [
        { component: TicTacToeGame, displayName: 'Tic-Tac-Toe' }
    ];

    let currentGame;
</script>

<GameSelector allGames={AVAILABLE_GAMES} bind:selectedGame={currentGame}/>

<GameArea game={currentGame}/>
