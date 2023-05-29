<script lang="ts">
	import type { element } from "svelte/internal";
    import Cell from "./Cell.svelte";
    import {State} from "./enums.svelte";
	import type { CellData } from "./types.svelte";
	import  {GameState} from "./enums.svelte";
	import Game from "./game.svelte";



    let board: CellData[][] = new Array();
    let id:number = 0;
    let currentGameState:GameState = GameState.PlayerTurn;

    for (let i = 0; i < 3; i++) {
        board[i] = new Array();
        for (let j = 0; j < 3; j++) {
            board[i][j] = {
                currentState: State.Empty,
                indexX: i,
                indexY: j
            };
            id++;
        }
    }
/*
    $: {
        board; //this block triggers when the board array is updated
        winner = hasWon();
    }
*/
    const runAi = ():void =>{
        console.log(Math.floor(Math.random() * (board[0].length - 1)));
        let flatBoard = board.flat();
        let emptyCells:CellData[] = flatBoard.filter(cell => cell.currentState === State.Empty);
        
        if (emptyCells.length === 0) {
            currentGameState = GameState.GameOver;
        }
        else{
            let rand = Math.floor(Math.random() * (emptyCells.length - 1));
            emptyCells[rand].currentState = State.Circle;
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
                currentGameState = GameState.GameOver;
            }
            if (horisontalSum === 6 || verticalSum === 6 || leftDiagonalSum === 6 || rightDiagonlSum === 6) {
                winner = "O";
                currentGameState = GameState.GameOver;
            }
        }
        return winner;
    }


    function onClick(event:Event):void {
        if (currentGameState !== GameState.GameOver) {
            changeCellState(event.target!);
            runAi();
            winner = hasWon();            
        }   
    }

    function changeCellState(eventTarget:EventTarget):void{
        let target:HTMLElement = eventTarget as HTMLElement;
        let targetIndexX = parseInt(target.dataset.indexX!, 10);
        let targetIndexY = parseInt(target.dataset.indexY!, 10);

        if (isNaN(targetIndexX) || isNaN(targetIndexY))
            throw new TypeError("Id is not a number");

        board[targetIndexX][targetIndexY].currentState = State.Cross;   
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
