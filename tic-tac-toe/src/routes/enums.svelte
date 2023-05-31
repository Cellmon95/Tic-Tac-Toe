<script lang="ts" context="module">
    export enum CellState{
        Cross,
        Empty,
        Circle,
    }

    export enum GameState{
        PlayerTurn,
        AITurn,
        GameOver
    }

    export enum GameResult{
        Undecided = "Undecided",
        Cross = "Cross",
        Circle = "Circle",
        Draw = "Draw",
    }

    export enum Difficulty{
        Easy,
        Normal,
        Hard,
    }
</script>