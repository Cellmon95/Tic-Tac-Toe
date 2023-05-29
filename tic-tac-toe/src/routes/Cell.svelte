<style>
    .cell{
        border: solid;
    }

    .cell img{
        width: 100%;
        height: 100%;
    }    
</style>

<script lang="ts" context="module">    

    let lastStone:State;
</script>

<script lang="ts">
    import {State} from "./enums.svelte";
    import crossImg from "../assets/img/cross.svg";
    import circleImg from "../assets/img/circle.svg";
    import type { CellData } from "./types.svelte";


    const statesSrc = new Map<State, string>([
        [State.Empty, ""],
        [State.Circle, circleImg],
        [State.Cross, crossImg]
    ]);

    let src:string = "";

    $: {
        src = statesSrc.get(cellData.currentState) as string;
    }

    export let cellData:CellData;

    
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="cell" on:click data-index-x={cellData.indexX} data-index-y={cellData.indexY}>
    {#if cellData.currentState !== State.Empty}
        <img {src} alt="cell">
    {/if}

</div>

