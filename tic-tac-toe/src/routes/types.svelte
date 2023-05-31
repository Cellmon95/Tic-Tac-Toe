<script lang="ts" context="module">
    import type { CellState } from "./enums.svelte";

    export type CellData = {
        currentState:CellState,
        indexX:number
        indexY:number
    }
</script>