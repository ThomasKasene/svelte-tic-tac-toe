<script>
    export let allGames = [];
    export let selectedGame = allGames[0];
</script>

<div class="wrapper">
{#each allGames as game, gameIndex}
    <input type="radio" bind:group={selectedGame} value={game} id={`game-selector-id-${gameIndex}`}/>
    <label for={`game-selector-id-${gameIndex}`}>{game.displayName}</label>
{/each}
</div>

<style>
.wrapper {
    background-color: black;
    border: 2px solid black;
    border-radius: 4px;
    display: grid;
    grid-gap: 2px;
    grid-auto-flow: column;
}

input {
    position: absolute;
    visibility: hidden;
}

label {
    background-color: #444;
    color: #b7b7aa;
    cursor: pointer;
    display: inline-block;
    font-size: 16pt;
    font-family: Helvetica, Arial;
    padding: 8px;
    text-align: center;
    user-select: none;
}

input:hover + label {
    background-color: #d7d7ca;
    color: #444;
}

input:checked + label {
    background-color: #b7b7aa;
    color: #444;
}
</style>