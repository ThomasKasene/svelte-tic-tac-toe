<script>
    import Board from './Board.svelte';
    import Button from './Button.svelte';
    import TurnIndicator from './TurnIndicator.svelte';
    import RulePanel from './RulePanel.svelte';

    export let m = 3;
    export let n = 3;
    export let k = 3;

    let players = [
        {
            name: 'Player 1',
            symbol: 'X',
            symbolColor: 'black',
            backgroundColor: '#F0C0C0',
            backgroundColorHover: '#F0D7D7',
            victoryCount: 0
        },
        {
            name: 'Player 2',
            symbol: 'O',
            symbolColor: 'black',
            backgroundColor: '#C0C0F0',
            backgroundColorHover: '#D7D7F0',
            victoryCount: 0
        }
    ];
    let currentPlayerIndex = 0;
    let currentPlayer = players[currentPlayerIndex];

    let board;
    let isGameOver = false;

    function onSquareMarked(event) {
        if (isGameOver) {
            return;
        }

        event.detail.markedSquare.claim(currentPlayer);

        if (hasVictory(event)) {
            isGameOver = true;
            incrementVictoryCountOfCurrentPlayer();
            return;
        } else if (isDraw()) {
            isGameOver = true;
            return;
        }
        changeTurn();
    }

    function hasVictory(event) {
        return event.detail.verticalStreak >= k
            || event.detail.horizontalStreak >= k
            || event.detail.diagonalStreak >= k;
    }

    function isDraw() {
        return board.getUnmarkedSquareCount() <= 0;
    }

    function incrementVictoryCountOfCurrentPlayer() {
        currentPlayer.victoryCount++;
        players = [...players];
    }

    function changeTurn() {
        currentPlayerIndex = currentPlayerIndex === 0 ? 1 : 0;
        currentPlayer = players[currentPlayerIndex];
    }

    function reset() {
        board.reset();
        changeTurn();
        isGameOver = false;
    }
</script>

<div style="display: grid">
    <RulePanel bind:m bind:n bind:k/>
    <TurnIndicator allPlayers={players} bind:currentPlayer/>

{#key m}
    {#key n}
        <Board
            bind:this={board}
            bind:width={m}
            bind:height={n}
            bind:currentPlayer
            on:squareclicked={onSquareMarked}
            />
            {#if isGameOver}
                <Button on:click={() => reset()}>Reset</Button>
            {/if}
    {/key}
{/key}

</div>
