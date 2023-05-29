<script lang="ts" context="module">
    enum GameState{
        PlayerTurn,
        AITurn,
        GameOver
    }
    export let currentGameState:GameState = GameState.PlayerTurn;
</script>