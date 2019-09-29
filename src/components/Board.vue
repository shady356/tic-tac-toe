<template>
  <div>
    <ul class="board">
      <li 
        v-for="tile in tiles" 
        :key="tile.id"  
        class="tile"
        @click="playerPlays(tile)"
      >
        <div 
          v-if="tile.played"
          :class="[ 'mark', (tile.playerMark === 'o') ? 'o' : 'x' ]"
        >
          {{tile.playerMark}}
        </div>
      </li>
    </ul>
    <button v-if="done" @click="resetGame">New Game</button>
  </div>
</template>

<script>
export default {
  name: 'board',
  data() {
    return {
      tiles: [],
      NUM_OF_TILES: 9,
      currentPlayer: 'x',
      takenTiles: [],

      done: false,

      isAIPlaying: false
    }
  },
  created() {
    this.tiles = this.generateTiles()
  },
  methods: {

    resetGame() {
      this.$emit('endGame', null )
      this.done = false
      this.takenTiles = []
      this.tiles = []
      this.currentPlayer = 'x'
      this.tiles = this.generateTiles()
    },
    generateTiles() {
      let tiles = []
      for(let i = 0; i < this.NUM_OF_TILES; i++) {
        tiles[i] = { 
          id: i,
          played: false,
          playerMark: ''
        }
      }
      return tiles
    },
    playerPlays(tile) {
      if(!this.isAIPlaying) {
        if( this.playTile(tile) ) {
          this.aiPlays()
        }
      }
    },
    playTile(tile) {
      if (!tile.played) {
        this.tiles[tile.id].playerMark = this.currentPlayer
        this.tiles[tile.id].played = true
        this.takenTiles.push(tile.id)
        if(!this.gameIsDone() && !this.done) {
          this.changePlayer()
          return true
        } else {
          this.done = true
          return false
        }
      }
    },
    aiPlays() {
      this.isAIPlaying = true
      let tileToPlay;
      
      
        tileToPlay = Math.floor((Math.random() * 8) + 0);
     
      setTimeout(()=>{
        if( this.canPlayTile(tileToPlay) ) {
          this.playTile(this.tiles[tileToPlay])
        }
        this.isAIPlaying = false
      }, 500)
      
    },
    canPlayTile(tile) {
      console.log(tile)
      if(this.takenTiles.includes(tile)) {
        this.aiPlays()
      } else {
        return true
      }
    },
    changePlayer() {
      this.currentPlayer = (this.currentPlayer === 'x') ? 'o' : 'x'
    },
    gameIsDone() {
      
      if ( this.checkGame('x') ) {
        this.$emit('endGame', 'Player X won!' )
        this.done = true
        return true
      }
      if ( this.checkGame('o') ) {
        this.$emit('endGame', 'Player O won!')
        this.done = true
        return true
      }
      this.done = false
      return false

    },
    checkGame (mark) {
      let ts = this.tiles;
       for (let i = 0; i < 8; i++) {
        if(i === 0) {
          // row
          if( ts[i].playerMark === mark && ts[i+1].playerMark === mark && ts[i+2].playerMark === mark ) {
            return true
          }
          //column
          if( ts[i].playerMark === mark && ts[i+3].playerMark === mark && ts[i+6].playerMark === mark ) {
            return true
          }
          //diagonal
          if( ts[i].playerMark === mark && ts[i+4].playerMark === mark && ts[i+8].playerMark === mark ) {
            return true
          }
        }
        else if(i === 2) {
          // column
          if( ts[i].playerMark === mark && ts[i+3].playerMark === mark && ts[i+6].playerMark === mark ) {
            return true
          }
          if( ts[i].playerMark === mark && ts[i+2].playerMark === mark && ts[i+4].playerMark === mark ) {
            return true
          }
        }
        else if(i === 4) {
          // column
          if( ts[i-3].playerMark === mark && ts[i].playerMark === mark && ts[i+3].playerMark === mark ) {
            return true
          }
          // row
          if( ts[i-1].playerMark === mark && ts[i].playerMark === mark && ts[i+1].playerMark === mark ) {
            return true
          }
        }
        if(i === 6) {
          // column
          if( ts[i].playerMark === mark && ts[i+1].playerMark === mark && ts[i+2].playerMark === mark ) {
            return true
          }
        }
        if(i === 7)
          return false
      }
    }
  }
}
</script>

<style scoped>
  .board {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    margin: 0 auto;

    width: 90vw;
    height: 90vw;
    
  }

  .tile {
    width: calc(100% / 3);
    height: calc(100% / 3);
    align-self: center;
    line-height: 100px;
    text-align: center;
    box-shadow: 1px 1px #555 inset;
    color: #aaa;
    font-size: 80px;
  }

  .tile:nth-child(1) {
    box-shadow: 0px 0px #555 inset;
  }
  .tile:nth-child(2), 
  .tile:nth-child(3) {
    box-shadow: 1px 0px #555 inset;
  }
  .tile:nth-child(4), 
  .tile:nth-child(7) {
    box-shadow: 0px 1px #555 inset;
  }

  .tile .mark.o {
    color: #00aeff;
  }
  .tile .mark.x {
    color: #d6a51d;
  }
  button {
    border: none;
    background: rgb(4, 139, 76);
    color: #fff;
    padding: 10px 20px;
    border-radius: 20px;
    font-size: 20px;
    margin: 80px auto 0;
  }
</style>