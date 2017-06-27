<template>
  <div>
    <div class="title">
      <h3>Tower of Hanoi Visualization</h3>
      <p>Click <a href="#">here</a> to learn how it's played</p>
    </div>
    <div class="puzzle">
      <div v-if="showWin">
        <p>You won in {{totalMoves}}</p>
      </div>
      <span v-if="totalMoves > 0">Moves made {{totalMoves}}</span>
      <button @click="restart">restart</button>
      <div class="flex-container">
        <div class="flex-item">
          <ul id="src">
            <li
              class="disk"
              v-for="disk in src"
              :key="disk.id"
              :class="{disk1: disk.id === disk1.id, disk3 : disk.id === disk3.id}"
              :style="{background: disk.color, width: disk.width}"
              @click="moveDisk(src,disk)">
              <span>{{disk.id}}</span>
            </li>
            <div
              class="base"
              @click="setDisk(getSource)"
              >source</div>
          </ul>
        </div>
        <div class="flex-item">
          <ul id="aux">
            <li
              class="disk"
              v-for="disk in aux"
              :key="disk.id"
              :class="{disk1: disk.id === disk1.id, disk3 : disk.id === disk3.id}"
              :style="{background: disk.color, width: disk.width}"
              @click="moveDisk(aux,disk)">
              <span>{{disk.id}}</span>
            </li>
            <div
              class="base"
              @click="setDisk(getAux)"
              >
              auxiliary
            </div>
          </ul>
          </div>
        <div class="flex-item">
          <ul id="dest">
            <li
              class="disk"
              v-for="disk in dest"
              :key="disk.id"
              :class="{disk1: disk.id === disk1.id, disk3 : disk.id === disk3.id}"
              :style="{background: disk.color, width: disk.width}"
              @click="moveDisk(dest,disk)">
              <span>{{disk.id}}</span>
            </li>
            <div
              class="base"
              @click="setDisk(getDest)"
              >
              destination
            </div>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      moves: 0,
      isEmpty: false,
      selectedDisk: '',
      fromPole: [],
      toPole: [],
      disk1: {id: 1, isTop: false, color: 'white', width: '50px'},
      disk2: {id: 2, isTop: false, color: '#41B883', width: '100px'},
      disk3: {id: 3, isTop: false, color: '#35495E', width: '150px'},
      src: [],
      aux: [],
      dest: []
    }
  },
  created () {
    this.stackDisks()
  },
  methods: {
    stackDisks () {
      this.src.unshift(this.disk3)
      this.src.unshift(this.disk2)
      this.disk1.isTop = true
      this.src.unshift(this.disk1)
    },
    restart () {
      this.src = []
      this.stackDisks()
      this.aux = []
      this.dest = []
      this.moves = 0
    },
    moveDisk (pole, disk) {
      console.log(`pole: ${pole} disk: ${disk}`)
      this.selectedDisk = disk
      this.fromPole = pole
    },
    setDisk (toPole) {
      this.toPole = toPole
      if (this.selectedDisk.isTop && this.isLegal()) {
        this.fromPole.map(theDisk => {
          if (theDisk.id === this.selectedDisk.id) {
            this.fromPole.shift()
            toPole.unshift(theDisk)
            this.setTop
          }
        })
        this.moves++
      } else {
        if (!this.selectedDisk.isTop) alert('cant move bottom disk')
        if (!this.isLegal()) alert('cant move larger disk on smaller disk')
      }
    },
    setTop () {
      this.toPole.forEach(disk => {
        if (disk.id !== this.toPole[0].id) {
          disk.isTop = false
        }
        this.toPole[0].isTop = true
      })
      this.fromPole.forEach(disk => {
        if (disk.id !== this.fromPole[0].id) {
          disk.isTop = false
        }
        this.fromPole[0].isTop = true
      })
    },
    isSmaller () {
      let sm = false
      this.toPole.map(disk => {
        if (this.selectedDisk.id < disk.id) {
          sm = true
        }
      })
      return sm
    },
    isLegal () {
      return this.toPole.length === 0 || this.isSmaller()
    }
  },
  computed: {
    showWin () {
      return this.src.length === 0 && this.aux.length === 0
    },
    totalMoves () {
      return this.moves
    },
    getSrc () {
      return this.src
    },
    getAux () {
      return this.aux
    },
    getDest () {
      return this.dest
    },
    numDisksOnSrc () {
      return this.src.length
    },
    numDisksOnAux () {
      return this.aux.length
    },
    numDisksOnDest () {
      return this.dest.length
    }
  }
}
</script>
<style>
  .flex-container {
    display: -webkit-flex;
    display: flex;
    border: 1px solid green;
    background-color: lightgrey;
    height: 250px;
    width: 800px;
  }
  .flex-item {
    background-color: #9E9E9E;
    width: 400px;
    height: 200px;
    margin: 30px;
  }
  .title {
    color: white;
    text-align: center;
  }
  .src .aux .dest {
    height: 222px;
    position: relative;
  }
  .base {
    border: 1px solid red;
    text-align: center;
    width: 350px;
    height: 25px;
    margin-left: -40px;
    position: absolute
  }
  .base:hover {
    cursor: pointer;
  }
  .disk {
    border: 1px solid red;
    height: 15px;
    text-align: center;
    padding: 10px;
    margin-left: 25%;
    border-radius: 15px;
    margin-top: 3px;
  }
  .disk:hover {
    cursor: pointer;
    background: green;
    color: white;
  }
  .disk1 {
    margin-left: 110px;
  }
  disk3 {
    margin-left: 60px;
  }
  ul {
    list-style: none;
  }
  a {
    color: white;
  }
  h4 {
    text-align: center;
    margin-right: 30px;
  }

</style>
