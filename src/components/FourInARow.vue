<template>
  <div>
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
      
      if(row > -1) {
        this.grid[row][column.column].isPlayed = this.currentPlayer.color

        // (!) TODO: Check game status
        
        this.changePlayer()
      } else {
        alert('Oh! it is already filled up!')
      }
    }
  }
}
</script>

<style scoped>

.grid {
  margin: 100px auto 0;
  color: #777;
  font-size: 12px;
  width: 500px;
  height: 500px;
}

.grid .rows {
  display: flex;
  flex-direction: row;
  height: calc(100% /5);
  color:
  
}

.grid .rows .columns {
  
  width: calc(100% / 5);
  border: 1px solid #444;
}

.ball {
  margin: 0 auto;
  margin-top: 10px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  animation: dropBall .4s ease-out;

}

.ball.orange {
  background: #e47207;
}
.ball.blue {
  background: #0793e4;
}

@keyframes dropBall {
  0% {
    transform: translateY(-20vh);
  }
  89% {
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