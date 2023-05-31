<script lang="ts">
    import type { element } from "svelte/internal";
    import Cell from "./Cell.svelte";
    import {CellState, GameState, GameResult, Difficulty} from "./enums.svelte";
	import type { CellData } from "./types.svelte";
	import Game from "./game.svelte";

    
    
    let board: CellData[][] = new Array();
    let id:number = 0;
    let currentGameState:GameState = GameState.SetDifficulty;
    let winner: GameResult = GameResult.Undecided;
    let currentDifficulty:Difficulty = Difficulty.Easy;

    for (let i = 0; i < 3; i++) {
        board[i] = new Array();
        for (let j = 0; j < 3; j++) {
            board[i][j] = {
                currentState: CellState.Empty,
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
    const runAi = (difficulty: Difficulty):void =>{
        //console.log(Math.floor(Math.random() * (board[0].length - 1)));
        let flatBoard = board.flat();
        let emptyCells:CellData[] = flatBoard.filter(cell => cell.currentState === CellState.Empty);
        
        if (emptyCells.length === 0) {
            currentGameState = GameState.GameOver;
        }
        else{
            let rand = Math.floor(Math.random() * (emptyCells.length - 1));
            let foundGoodMove:boolean = false;
            switch (difficulty){
                case Difficulty.Easy:
                    emptyCells[rand].currentState = CellState.Circle;
                    break;

                case Difficulty.Normal:
                    
                    foundGoodMove = findGoodMove("block");

                    if (!foundGoodMove) {
                        emptyCells[rand].currentState = CellState.Circle;
                    }
                    break;
                    
                case Difficulty.Hard:

                    foundGoodMove = findGoodMove("win");
                    if (!foundGoodMove) {
                        foundGoodMove = findGoodMove("block");
                    }

                    let emptyCornerCells = emptyCells.filter(cell => (cell.indexX !== 1 && cell.indexY !== 1));
                    let emptyEdgeCells = emptyCells.filter(cell => (cell.indexX === 1 || cell.indexY === 1));
                    if (!foundGoodMove) {
                        if (board[1][1].currentState === CellState.Empty) {
                            board[1][1].currentState = CellState.Circle;
                        } else if (board[1][1].currentState === CellState.Cross && emptyCornerCells.length > 0) {
                            rand = Math.floor(Math.random() * (emptyCornerCells.length - 1));
                            emptyCornerCells[rand].currentState = CellState.Circle;
                        } else if (emptyEdgeCells.length > 0) {
                            rand = Math.floor(Math.random() * (emptyEdgeCells.length - 1));
                            emptyEdgeCells[rand].currentState = CellState.Circle;
                        } else {
                            emptyCells[rand].currentState = CellState.Circle;
                        }
                    }
                    break;
            }
        }
    }

    const findGoodMove = (goal: string): boolean => {
        let foundGoodMove:boolean = false;
        board.forEach(row => {
            let horisontalSum:number = 0;
            row.forEach(cell => {
                horisontalSum += cell.currentState;
            });
            if(placeGoodMove(row, horisontalSum, goal)) {
                foundGoodMove = true;
            }
            
        });
        if (!foundGoodMove){
            for (let i = 0; i < board.length; i++) {
                let verticalSum:number = 0;
                for (let j = 0; j < board.length; j++) {
                    verticalSum += board[j][i].currentState;
                }
                if(placeGoodMove([board[0][i], board[1][i], board[2][i]], verticalSum, goal)){
                    foundGoodMove = true;
                }

            } 
        }
        if (!foundGoodMove){
            for (let i = 0; i < board.length; i++) {
                let verticalSum:number = 0;
                for (let j = 0; j < board.length; j++) {
                    verticalSum += board[j][j].currentState;
                }
                if(placeGoodMove([board[0][0], board[1][1], board[2][2]], verticalSum, goal)){
                    foundGoodMove = true;
                }

            } 
        }
        if (!foundGoodMove){
            for (let i = 0; i < board.length; i++) {
                let verticalSum:number = 0;
                for (let j = 0; j < board.length; j++) {
                    verticalSum += board[2-j][j].currentState;
                }
                if(placeGoodMove([board[2][0], board[1][1], board[0][2]], verticalSum, goal)){
                    foundGoodMove = true;
                    
                }

            } 
        }
        return foundGoodMove;
    }

    const placeGoodMove = (line: CellData[], sum: number, goal: string): boolean => {
        if (sum === 5 && goal === "win") {
            line.filter(cell => cell.currentState === CellState.Empty)[0].currentState = CellState.Circle;
            return true;
        }
        if (sum === 1 && goal === "block") {
            line.filter(cell => cell.currentState === CellState.Empty)[0].currentState = CellState.Circle;
            return true;
        }
        return false;
    }

    const hasWon = (): GameResult => {
        let winner: GameResult = GameResult.Undecided;
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
                winner = GameResult.Cross;
                currentGameState = GameState.GameOver;
            }
            if (horisontalSum === 6 || verticalSum === 6 || leftDiagonalSum === 6 || rightDiagonlSum === 6) {
                winner = GameResult.Circle;
                currentGameState = GameState.GameOver;
            }
        } 
        
        if (currentGameState === GameState.GameOver && winner === GameResult.Undecided) {
            winner  = GameResult.Draw;
        }

        return winner;
    }


    function onCellClick(event:Event):void {
        if (currentGameState !== GameState.GameOver) {
            changeCellState(event.target!);          
            winner = hasWon();
        }
        if (currentGameState !== GameState.GameOver) {
            runAi(currentDifficulty);
            winner = hasWon();        
        }   
    }

    function onDifficultyClick(event:Event) {
        let selectedDifficulty:string | undefined = (event.target as HTMLElement).dataset.difficulty;

        currentGameState = GameState.PlayerTurn;

        switch (selectedDifficulty) {
            case "easy":
                currentDifficulty = Difficulty.Easy;
                break;
            case "normal":
                currentDifficulty = Difficulty.Normal;
            break;
            case "hard":
                currentDifficulty = Difficulty.Hard;
            break;
        
            default:
                throw TypeError("unexpected value in selected difficulty.")
        }
    }

    function changeCellState(eventTarget:EventTarget):void{
        let target:HTMLElement = eventTarget as HTMLElement;
        let targetIndexX:number = parseInt(target.dataset.indexX!, 10);
        let targetIndexY:number = parseInt(target.dataset.indexY!, 10);

        if (isNaN(targetIndexX) || isNaN(targetIndexY))
            throw new TypeError("Id is not a number");

        board[targetIndexX][targetIndexY].currentState = CellState.Cross;   
    }

</script>

<main>

    {#if currentGameState === GameState.SetDifficulty}
        <div class="difficulty-settings">
            <h1>Select Difficulty</h1>
            <button on:click={onDifficultyClick} data-difficulty="easy">Easy</button>
            <button on:click={onDifficultyClick} data-difficulty="normal">Normal</button>
            <button on:click={onDifficultyClick} data-difficulty="hard">Impossible</button>
        </div>
    {:else}
        <div class="grid">
            {#each board as row}
                {#each row as cell}
                    <Cell bind:cellData={cell}  on:click={onCellClick}/>
                {/each}
            {/each}
        </div>
        <h1>Winner: {winner}</h1>
    {/if}

</main>
