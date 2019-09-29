<template>
  <div id="app">
   
    <board
      class="board"
      @endGame="showEndGame"
    />

    <div class="showEndGame" v-if="isEndGame">
      {{endGameText}}
    </div>
  
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Board from './components/Board.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    Board
  },
  data() {
    return {
      isEndGame: false,
      endGameText: ""
    }
  },
  methods: {
    showEndGame(endGameText) {
      this.isEndGame = true
      this.endGameText = endGameText
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}
*{
  padding: 0;
  margin: 0;
  list-style: none;
}
body {
  background: #222;
}

.board {
  padding-top: 20px;
}

.showEndGame {
  text-align: center;
  color: #fff;
  font-size: 36px;
  margin-top: 20px;
}
</style>
