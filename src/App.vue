<template>
  <div id="app">
   
    <board/>
  
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
</style>
