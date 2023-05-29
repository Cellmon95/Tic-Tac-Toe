<script lang="ts">
    import Cell from "./Cell.svelte";
    import { onMount } from 'svelte';
    import {State} from "./enums.svelte";

    let board: State[][] = [
        [State.Empty, State.Empty, State.Empty],
        [State.Empty, State.Empty, State.Empty],
        [State.Empty, State.Empty, State.Empty],
    ];

    $: {
        board; //this block triggers when the board array is updated
        winner = hasWon();
    }

    const hasWon = (): string => {
        let winner: string = "Undecided";
        let boardFilled = true;
        for (let i = 0; i < board.length; i++) {
            let horisontalSum:number = 0;
            let verticalSum:number = 0;
            let leftDiagonalSum:number = 0;
            let rightDiagonlSum:number = 0;
            for (let j = 0; j < board.length; j++) {
                horisontalSum += board[i][j];
                verticalSum += board[j][i];
                leftDiagonalSum += board[j][j];
                rightDiagonlSum += board[2-j][j];
                if (board[i][j] === 1) {
                    boardFilled = false;
                }
            }
            if (horisontalSum === 0 || verticalSum === 0 || leftDiagonalSum === 0 || rightDiagonlSum === 0) {
                winner = "X";
            }
            else if (horisontalSum === 6 || verticalSum === 6 || leftDiagonalSum === 6 || rightDiagonlSum === 6) {
                winner = "O";
            }else if (boardFilled) {
                winner = "Draw";
            }
        }
        return winner;
    }

    let winner: string = "Undecided";
</script>

<main>
    
    <div class="grid">
        {#each board as row}
            {#each row as cell}
                <Cell bind:currentState={cell}/>
            {/each}
        {/each}
    </div>
    <h1>Winner: {winner}</h1>
</main>
