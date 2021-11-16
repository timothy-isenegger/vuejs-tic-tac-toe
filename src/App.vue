<template>
  <div id="app">
    <h1>Tic Tac Toe</h1>
    <GameBoard></GameBoard>
  </div>
</template>

<script>
import GameBoard from './components/GameBoard.vue';
export default {
  name: 'App',
  components: {
    GameBoard
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 300px;
  width: 100%;
}

:root {
  --text-color: #2c3e50;
  --text-color-inactive: #c9c9c9;
  --bg-color: #fff;
  --bg-color-hover: rgba(44, 62, 80, .3);
  --bg-color-inactive: #f0f0f0;
}
</style>
