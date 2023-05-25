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
    enum State{
        Cross,
        Empty,
        Circle,
    }

    let lastStone:State;
</script>

<script lang="ts">
    import { empty } from "svelte/internal";
    import cross from "../assets/img/cross.svg";
    import circle from "../assets/img/circle.svg";


    let src:string = "";
    export let currentState:State = State.Empty;

    function onClick(event:Event):void {
        if (currentState === State.Empty) {
            if (lastStone === State.Cross){
                src = circle;
                currentState = State.Circle;
            } else {
                src = cross;
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

