<template>
  <div id="app">
   
    <!-- <board
      class="board"
      @endGame="showEndGame"
      @comThinking="updateComThinking"
    /> -->

    <FourInARow />
    
    <div class="msg">
      {{msg}}
    </div>
  
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import FourInARow from './components/FourInARow.vue'
//import Board from './components/Board.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    FourInARow
   // Board
  },
  data() {
    return {
      isEndGame: false,
      msg: ""
    }
  },
  methods: {
    showEndGame(endGameText) {
      this.isEndGame = true
      this.msg = endGameText
    },
    updateComThinking(text) {
      this.msg = text
    }
  }
}
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
}
*{
  padding: 0;
  margin: 0;
  list-style: none;
  font-family: 'Roboto', sans-serif;
}
body {
  background: #222;
}

.board {
  padding-top: 20px;
}

.msg {
  text-align: center;
  color: #fff;
  font-size: 36px;
  margin-top: 20px;
}
</style>
