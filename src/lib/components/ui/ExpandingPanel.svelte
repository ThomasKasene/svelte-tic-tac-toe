<script>
    import { slide } from 'svelte/transition';

    export let title = 'Panel Title';
    export let expanded = true;

    function expandOrCollapse() {
        expanded = !expanded;
    }
</script>

<div class="panelWrapper">
    <div class="panelHeader" on:click={() => expandOrCollapse()} on:keyup={() => expandOrCollapse()}>{title}</div>
{#if expanded}
    <div class="panelContents" transition:slide><slot/></div>
{/if}
</div>

<style>
    .panelWrapper {
        margin-bottom: 10px;
    }

    .panelHeader {
        background-color: #d7d7ca;
        border-color: #b7b7aa;
        border-style: solid;
        border-width: 1px;
        border-radius: 6px;
        font-family: Helvetica, Arial;
        padding: 8px 16px;
    }

    .panelContents {
        background-color: #eee;
        border-color: #ddd;
        border-style: solid;
        border-bottom-left-radius: 4px;
        border-bottom-right-radius: 4px;
        border-width: 0px 1px 1px 1px;
        font-family: Helvetica, Arial;
        margin: 0px 6px;
        padding: 8px;
    }
</style>