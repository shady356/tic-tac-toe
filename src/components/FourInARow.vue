<template>
  <div class="frame">
   <!-- Grid -->

   <div class="grid">
    <!-- Rows -->
      <div 
        v-for="(row, index) in grid" 
        :key="index"
        class="rows"
      > 
        <!-- slots -->
        <div 
          v-for="column in row" 
          :key="column.column"
          class="columns"
          @click="playColumn(column)"
        >
          {{column.row +',' + column.column}}
          <div 
            v-if="column.isPlayed.length > 0" 
            :class="['ball', column.isPlayed]"
          />
        </div>

      </div>
   </div>

  </div>
</template>

<script>
export default {
  name: 'Four-in-a-row',
  data() {
    return {
      p1: {
        id: 1,
        name: 'Player 1',
        color: 'blue',
        score: 0
      },
      p2: {
        id: 2,
        name: 'Player 2',
        color: 'orange',
        score: 0
      },
      currentPlayer: null,
      grid: [],
      gridTakenSlots : [],
      gridSizeRow: 5,
      gridSizeColumn: 5
    }
  },
  created() {
    this.grid = this.generateGrid() 
    this.currentPlayer = this.p1
  },
  methods: {
    generateGrid() {
      let grid = []
      for (let r = 0; r < this.gridSizeRow; r++) {
        grid[r] = []

        for (let c = 0; c < this.gridSizeColumn; c ++) {
          grid[r][c] = {
            row: r,
            column: c,
            isPlayed: ""
          }
        }
      }
      return grid
    },
    changePlayer() {
      this.currentPlayer = (this.currentPlayer.id === 1) ? this.p2 : this.p1
    },
    canPlayColumn(c) {
      for(let i = this.gridSizeColumn-1; i >= 0; i--) {
        if(!this.grid[i][c].isPlayed) {
          return i
        }
      }
      return -1
    },
    playColumn(column) {
      let row = this.canPlayColumn(column.column)
      const playerColor = this.currentPlayer.color
   
      if(row >= 0) {
        this.grid[row][column.column].isPlayed = playerColor

        this.isGame( {row:row, column:column.column, playerColor} )
        
        this.changePlayer()
      } else {
        alert('Oh! it is already filled up!')
      }
    },
    isGame(token) {
      const tr = token.row
      const tc = token.column
      const pc = token.playerColor
      const g = this.grid 
      
      //this.isHorizontal(tr,tc,pc,g)
     
  
    },
    /* isHorizontal(tr,tc,pc,g) {
      //  0, 1, 2, 3, 4
      let diff = this.gridSizeRow - tc // 5 - 5 = 0 || 
      if(tc > 0)
        
        if ( g[tr][tc].isPlayed === tc && g[tr][tc] === )
    } */
  }
}
</script>

<style scoped>

.frame {
  display: block;
  background-image: url('../assets/frame.png');
  margin: 100px auto 0;
  height: 600px;
  width: 635px;
}

.grid {
  position: relative;
  top: 34px;
  margin: 0 auto;
  color: #777;
  font-size: 12px;
  width: 500px;
  height: 500px;
  z-index: 100;
}

.grid .rows {
  display: flex;
  flex-direction: row;
  height: calc(100% /5);
  color:
  
}

.grid .rows .columns {
  
  width: calc(100% / 5);
  border: 1px solid #947e59;
  background-image: url("../assets/tile.png");
  background-position: center; 
  

}

.ball {
  position: relative;
  margin: 0 auto;
  width: 75px;
  height: 75px;
  border-radius: 50%;
  z-index: -1;
  animation: dropBall 1000ms cubic-bezier(0.620, 0.150, 0.080, 0.980);
}

.ball.orange {
  background-image: url('../assets/orangeBall.png')
}
.ball.blue {
  background-image: url('../assets/blueBall.png')
}

@keyframes dropBall {
  0% {
    transform: translateY(-70vh);
  }
  85% {
    transform: translateY(0px);
  }
  90% {
    transform: translateY(-4px);
  }
  100% {
    transform: translateY(0px);
  }
}

</style>