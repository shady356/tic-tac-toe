<template>
  <div>
    <ul class="board">
      <li 
        v-for="tile in tiles" 
        :key="tile.id"  
        class="tile"
        @click="playerPlays(tile)"
      >
        <div v-if="tile.played">
          {{tile.playerMark}}
        </div>
      </li>
    </ul>
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
      gameStatus: {
        gameIsDone: false,
      }
    }
  },
  created() {
    this.tiles = this.generateTiles()
  },
  methods: {
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
      this.playTile(tile)
      this.aiPlays()
    },
    playTile(tile) {
      if (!tile.played) {
        this.tiles[tile.id].playerMark = this.currentPlayer
        this.tiles[tile.id].played = true
        this.takenTiles.push(tile.id)
        this.gameIsDone()
        this.changePlayer()
      }
    },
    aiPlays() {

      let tileToPlay = Math.floor((Math.random() * 8) + 0);
      console.log(tileToPlay)

      if( this.canPlayTile(tileToPlay) ) {
        this.playTile(this.tiles[tileToPlay])
      }
      
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
        alert('x won')
        this.generateTiles()
      }
      if ( this.checkGame('o') ) {
        alert('o won')
        this.generateTiles()
      }
      

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
    background: #111;
    width: 90vw;
    height: 90vw;
    border: 1px solid #444;
  }

  .tile {
    width: calc(100% / 3);
    height: calc(100% / 3);
    align-self: center;
    text-align: center;
    box-shadow: 1px 1px #555 inset;
    color: #aaa;
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
</style>