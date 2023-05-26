<style>
    .cell{
        border: solid;
    }

    .cell img{
        width: 100%;
        height: 100%;
    }    
</style>

<script lang="ts">
    import {States} from "./enums.svelte";
    import crossImg from "../assets/img/cross.svg";
    import circleImg from "../assets/img/circle.svg";

    const statesSrc = new Map<States, string>([
        [States.Empty, ""],
        [States.Circle, circleImg],
        [States.Cross, crossImg]
    ]);
    let src:string = "";
    export let currentState:States = States.Empty;

    $: {
        src = statesSrc.get(currentState) as string;
    }
    

    function onClick(event:Event):void {
        if (currentState === States.Empty) {
            currentState = States.Cross;
        }
    }

</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="cell" on:click={onClick}>
    {#if currentState !== States.Empty}
        <img {src} alt="cell">
    {/if}

</div>

