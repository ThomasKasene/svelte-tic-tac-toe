<script>
    import Board from './Board.svelte';
    import Button from './Button.svelte';
    import TurnIndicator from './TurnIndicator.svelte';
    import ExpandingPanel from './ExpandingPanel.svelte';
    import NumericSliderRule from './NumericSliderRule.svelte';

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

    <TurnIndicator allPlayers={players} currentPlayer={currentPlayer}/>

    <ExpandingPanel title="Game Rules" style="display: grid">
        <NumericSliderRule bind:value={m} min="3" max="9" label="The horizontal size of the board."/>
        <NumericSliderRule bind:value={n} min="3" max="9" label="The vertical size of the board."/>
        <NumericSliderRule bind:value={k} min="3" max="5" label="How many in a row one needs to win."/>
    </ExpandingPanel>

    <Board
        bind:this={board}
        width={m}
        height={n}
        currentPlayer={currentPlayer}
        on:squareclicked={onSquareMarked}
        />

{#if isGameOver}
    <Button on:click={() => reset()}>Reset</Button>
{/if}

</div>
