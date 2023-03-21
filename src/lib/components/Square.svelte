<script>
    let owner = null;

    export function claim(player) {
        owner = player;
    }

    export function isClaimed() {
        return owner != null;
    }

    export function isOwnedBy(player) {
        return owner != null && owner === player;
    }

    export function reset() {
        owner = null;
    }
</script>

<button on:click
        style:background-color={isClaimed() ? owner.backgroundColor : ''}
        style:color={isClaimed() ? owner.symbolColor : ''}
>{isClaimed() ? owner.symbol : ''}</button>

<style>
    button {
        width: 100%;
        height: 100%;
        margin: 0px;
        padding: 0px;
        border-radius: 4px;
        border-color: #000000;
        font-size: 32pt;
        vertical-align: top;
        background-color: var(--bg-color, #D7D7D7);
    }

    button:hover, button:focus {
        background-color: var(--bg-color-hover, #E7E7E7);
    }
</style>
