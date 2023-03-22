<script>
    import newUniqueId from 'locally-unique-id-generator';

    export let label;
    export let min;
    export let max;
    export let value;

    let id = newUniqueId();
</script>

<div class="ruleWrapper">
    <div class="labelWrapper">
        <label for={id}>{label}</label>
    </div>
    <div class="controlWrapper">
        <div>
            <input type="range" id={id} min={min} max={max} bind:value/>
        </div>
        <div class="valueWrapper">
            <span>{value}</span>
        </div>
    </div>
</div>

<style>
    .ruleWrapper {
        margin: 10px 0px 20px 0px;
    }

    .labelWrapper {
        margin-bottom: 6px;
    }

    .controlWrapper {
        display: grid;
        grid-auto-flow: column;
        grid-auto-columns: max-content;
    }

    .valueWrapper {
        margin-left: 10px;
    }
</style>