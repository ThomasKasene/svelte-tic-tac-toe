<script>
    export let game;
</script>

<div>
{#if game}
    <h1>{game.displayName}</h1>
    <svelte:component this={game.component}/>
{:else}
    <h1>No game selected</h1>
{/if}
</div>

<style>
</style>