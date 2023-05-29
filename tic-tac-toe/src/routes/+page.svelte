<script lang="ts">
	import type { element } from "svelte/internal";
    import Cell from "./Cell.svelte";
    import {State} from "./enums.svelte";
	import type { CellData } from "./types.svelte";



    let board: CellData[][] = new Array();
    let id:number = 0;
    for (let i = 0; i < 3; i++) {
        board[i] = new Array();
        for (let j = 0; j < 3; j++) {
            board[i][j] = {
                currentState: State.Empty,
                id: id
            };
            id++;
        }
    }

    $: {
        board; //this block triggers when the board array is updated
        winner = hasWon();
    }

    const runAi = ():void =>{
        let running:boolean = true;
        let randRow:number = Math.floor(Math.random() * board.length - 1);
        let randColumn:number = Math.floor(Math.random() * board[0].length - 1);

        while(running){
            
            if (board[randRow][randColumn].currentState === State.Empty) {
                board[randRow][randColumn].currentState = State.Circle;
                running = false;
                break;
            }

            randRow = Math.floor(Math.random() * board.length - 1);
            randColumn = Math.floor(Math.random() * board[0].length-  1);
        }
    }

    const hasWon = (): string => {
        let winner: string = "Undecided";
        for (let i = 0; i < board.length; i++) {
            let horisontalSum:number = 0;
            let verticalSum:number = 0;
            let leftDiagonalSum:number = 0;
            let rightDiagonlSum:number = 0;
            for (let j = 0; j < board.length; j++) {
                horisontalSum += board[i][j].currentState;
                verticalSum += board[j][i].currentState;
                leftDiagonalSum += board[j][j].currentState;
                rightDiagonlSum += board[2-j][j].currentState;
            }
            if (horisontalSum === 0 || verticalSum === 0 || leftDiagonalSum === 0 || rightDiagonlSum === 0) {
                winner = "X";
            }
            if (horisontalSum === 6 || verticalSum === 6 || leftDiagonalSum === 6 || rightDiagonlSum === 6) {
                winner = "O"
            }
        }
        return winner;
    }


    function onClick(event:Event):void {   
        getCellData(event.target!);
    }

    //TODO: rename
    function getCellData(eventTarget:EventTarget):void{
        let target:HTMLElement = eventTarget as HTMLElement;
        let targetId = parseInt(target.dataset.id!, 10);

        if (isNaN(targetId))
            throw new TypeError("Id is not a number");
            for (let i = 0; i < board.length; i++) {
                for (let j = 0; j < board[i].length; j++) {
                    if (board[i][j].id === targetId) {
                        board[i][j].currentState = State.Cross;                        
                    }
                }
                
            }
    }

    let winner: string = "Undecided";
</script>

<main>
    
    <div class="grid">
        {#each board as row}
            {#each row as cell}
                <Cell bind:cellData={cell}  on:click={onClick}/>
            {/each}
        {/each}
    </div>
    <h1>Winner: {winner}</h1>
</main>
