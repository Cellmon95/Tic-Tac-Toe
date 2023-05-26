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

    const statesSrc = new Map<State, string>([
        [State.Empty, ""],
        [State.Circle, circleImg],
        [State.Cross, crossImg]
    ]);
    $: {
        src = statesSrc.get(currentState) as string;
    }
    let src:string = "";
    export let currentState:State = State.Empty;
    $: {
        src = statesSrc.get(currentState) as string;
    }
    

    function onClick(event:Event):void {
        if (currentState === State.Empty) {
            if (lastStone === State.Cross){
                currentState = State.Circle;
            } else {
                currentState = State.Cross;
            }
            lastStone = currentState;
            
        }
    }

    
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="cell" on:click={onClick}>
    {#if currentState !== State.Empty}
        <img {src} alt="cell">
    {/if}

</div>

