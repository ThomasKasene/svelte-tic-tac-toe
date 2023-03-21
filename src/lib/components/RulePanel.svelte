<script>
    export let m;
    export let n;
    export let k;
</script>

<div>
    <div>
        <input id="m" type="range" min="3" max="9" bind:value={m}/>
        <label for="m">m: {m}</label>
    </div>
    <div>
        <input id="n" type="range" min="3" max="9" bind:value={n}/>
        <label for="n">n: {n}</label>
    </div>
    <div>
        <input id="k" type="range" min="3" max="5" bind:value={k}/>
        <label for="k">k: {k}</label>
    </div>
</div>

<style>
</style>