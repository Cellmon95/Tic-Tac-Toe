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
    import { empty } from "svelte/internal";
import cross from "../assets/img/cross.svg";

    enum States{
        Cross,
        Circle,
        Empty
    }

    let src:string = "";
    let currentState:States = States.Empty;
    

    function onClick(event:Event):void {
        if (currentState === States.Empty) {
            src = cross;
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

