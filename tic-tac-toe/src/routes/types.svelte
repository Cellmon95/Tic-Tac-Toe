<script lang="ts" context="module">
    import type { State } from "./enums.svelte";

    export type CellData = {
        currentState:State,
        indexX:number
        indexY:number
    }
</script>