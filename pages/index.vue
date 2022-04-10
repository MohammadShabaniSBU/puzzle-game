<template>
  <div class="w-screen h-screen flex flex-col gap-2 justify-center items-center">
    <div v-if="won" class="text-white bg-green-500 py-4 px-6 rounded-full ">
      YOU WON
    </div>
    <div>
      <button @click="shuffle" class="bg-blue-500 text-white rounded px-5 py-2 ">
        Shuffle
      </button>
    </div>
    <div class="w-96 bg-gray-300 rounded p-0 m-0" style="aspect-ratio: 1 / 1;">
      <div class="relative h-full">
        <div 
          @click="handle(cell)" v-for="cell of cells" :key="cell" 
          class="p-2 w-1/4 absolute transition-all duration-300 ease-in" 
          style="aspect-ratio: 1 / 1;" 
          :style="{left: cell.left, top: cell.top}" 
        >
          <div class="w-full h-full rounded bg-red-500 flex justify-center items-center text-white">
            <div class="text-2xl font-bold">
              {{ cell.id + 1 }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      countOfCells: 15,
      cells: [],
      emptyPostion: 15,
      won: false,
    }
  },
  created() {
    for (let i = 0; i < this.countOfCells; i++)
      this.cells.push({
        id: i,
        left: (i % 4) * 25 + '%',
        top: Math.floor(i / 4) * 25 + '%',
        position: i
      });
  },
  head() {
    return {
      title: 'Puzzle Game'
    }
  },
  methods: {
    handle(cell) {
      this.move(cell)
      this.checkWon()
    },
    move(cell) {
      let pos = cell.position;
      let dis = this.emptyPostion - pos;
      switch (dis) {
        case 1:
          this.xTrnasform(cell, 1)
          break;
        case -1:
          this.xTrnasform(cell, -1)
          break;
        case 4:
          this.yTrnasform(cell, 1)
          break;
        case -4:
          this.yTrnasform(cell, -1)
          break;
      }
    },
    yTrnasform(cell, dir) {
      let num = cell.top
      cell.top = parseInt(num.substr(0, num.length - 1)) + (dir * 25) + '%';

      this.setEmptyPos(cell)
    },

    xTrnasform(cell, dir) {
      let num = cell.left
      cell.left = parseInt(num.substr(0, num.length - 1)) + (dir * 25) + '%';

      this.setEmptyPos(cell)
    },

    setEmptyPos(cell) {
      let tmp = this.emptyPostion;
      this.emptyPostion = cell.position
      cell.position = tmp
    },
    
    shuffle() {
      for (let i = 0; i < this.countOfCells; i++) {
        let a = Math.floor(Math.random() * this.countOfCells)
        let b = Math.floor(Math.random() * this.countOfCells)
        this.swap(this.cells[a], this.cells[b])
      }

      this.won = false;
    },

    swap(a, b) {
      let {left, top, position} = a
      a.left = b.left
      a.top = b.top
      a.position = b.position
      b.top = top
      b.left = left
      b.position = position
    },
    checkWon() {
      for(let cell of this.cells) {
        if (cell.id != cell.position) {
          this.won = false;
          return;
        }
      }
      this.won = true
    }
  }
}
</script>
