<script>
import Cell from "./Cell.svelte";
import {States} from "./enums.svelte";
</script>

<main>
    <div class="grid">
        <Cell currentState={States.Cross}/>
        <Cell/>
        <Cell/>
        <Cell/>
        <Cell/>
        <Cell/>
        <Cell/>
        <Cell/>
        <Cell/>
    </div>
</main>
