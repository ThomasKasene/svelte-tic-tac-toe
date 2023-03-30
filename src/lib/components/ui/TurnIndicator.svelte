<script>
    export let allPlayers;
    export let currentPlayer = null;
</script>

<div class="wrapper">
    {#each allPlayers as player}
        <div class="playerDiplay">
            <div class="turnIndicator" style:visibility={currentPlayer === player ? 'visible' : 'hidden'}>
                ⬤
            </div><div class="playerName">
                {player.name}
            </div><div class="playerVictories">
                ♔ {player.victoryCount}
            </div>
        </div>
    {/each}
</div>

<style>
    div.wrapper {
        display: grid;
        grid-auto-flow: row;
        grid-gap: 8px;
        border: 2px solid black;
        border-radius: 8px;
        margin: 8px 0px;
        padding: 8px;
        background-color: #DDDDFF;
    }

    div.playerDiplay {
        border-radius: 4px;
        display: grid;
        grid-auto-flow: column;
        background-color: white;
    }

    div.playerDiplay div {
        padding: 4px 8px;
    }
</style>