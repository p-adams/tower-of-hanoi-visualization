<template>
  <div>
    <div class="title">
      <h3>Tower of Hanoi Visualization</h3>
      <p v-if="!showInstructions">Click <a href="#" @click="handleOpen">here</a> to learn how it's played</p>
      <p class="inst" v-if="showInstructions">The object of the game is to move all the disks over to 
      the destination tower. Each disk is moved by clicking on the disk
      and then the pole upon which you want to place the disk.
      You can only move one disk at a time and you cannot place a larger disk onto a smaller disk. <a href="#" @click="handleClose">close to play!</a></p>
    </div>
    <div v-if="!showInstructions" class="puzzle">
      <div class="win" v-if="showWin">
        <h3>You won in {{totalMoves}} moves!</h3>
      </div>
      <p class="moves" v-if="totalMoves > 0">Moves made {{totalMoves}}</p>
      <button @click="restart">restart</button>
      <div class="flex-container">
        <div class="flex-item">
          <ul class="src">
            <div class="pole">
              <li
                class="disk"
                v-for="disk in src"
                :key="disk.id"
                :class="{disk1: disk.id === disk1.id, disk2: disk.id === disk2.id, disk3 : disk.id === disk3.id}"
                :style="{background: disk.color, width: disk.width}"
                @click="moveDisk(src,disk)">
              </li>
              </div>
            <div
              class="base"
              @click="setDisk(getSrc)">
            <h3>source</h3>
            </div>
          </ul>
        </div>
        <div class="flex-item">
          <ul class="aux">
            <div class="pole">
              <li
                class="disk"
                v-for="disk in aux"
                :key="disk.id"
                :class="{disk1: disk.id === disk1.id, disk2: disk.id === disk2.id, disk3 : disk.id === disk3.id}"
                :style="{background: disk.color, width: disk.width}"
                @click="moveDisk(aux,disk)">
              </li>
            </div>
            <div
              class="base"
              @click="setDisk(getAux)"
              >
              <h3>auxiliary</h3>
            </div>
          </ul>
          </div>
        <div class="flex-item">
          <ul class="dest">
            <div class="pole">
              <li
                class="disk"
                v-for="disk in dest"
                :key="disk.id"
                :class="{disk1: disk.id === disk1.id, disk2: disk.id === disk2.id, disk3 : disk.id === disk3.id}"
                :style="{background: disk.color, width: disk.width}"
                @click="moveDisk(dest,disk)">
              </li>
            </div>
            <div
              class="base"
              @click="setDisk(getDest)"
              >
              <h3>destination</h3>
            </div>
          </ul>
        </div>
      </div>
      <transition name="fade">
      <div v-if="errorMsg1">
        <p class="error">Cannot move bottom disk</p>
      </div>
      <div v-else-if="errorMsg2">
        <p class="error">Cannot place a larger disk onto a smaller disk</p>
      </div>
      </transition>

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
      dest: [],
      showInstructions: false,
      errorMsg1: false,
      errorMsg2: false
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
      [this.errorMsg1, this.errorMsg2] = [false, false]
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
            this.setTop()
          }
        })
        this.moves++
      } else {
        if (!this.selectedDisk.isTop) this.errorMsg1 = true
        else if (!this.isLegal()) this.errorMsg2 = true
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
      if (this.fromPole[0]) this.fromPole[0].isTop = true
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
    },
    toggleInstruction () {
      this.showInstructions = !this.showInstructions
    },
    handleOpen () {
      this.showInstructions = true
    },
    handleClose () {
      this.showInstructions = false
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
    width: 1000px;
    margin-top: 35px;
  }
  .flex-item {
    background-color: #9E9E9E;
    width: 700px;
    height: 200px;
    margin: 10px;
    position: relative;
  }
  .pole {
    height: 150px;
    width: 215px;
    margin-top: 0.7cm;
    position: absolute;
  }
  .title {
    color: white;
    text-align: center;
  }
  .src, .aux, .dest {
    width: 275px;
    height: 185px;
    position: relative;
  }
  .base {
    background: #333333;
    color: white;
    text-align: center;
    width: 314px;
    height: 35px;
    margin-left: -40px;
    bottom: 0;
    position: absolute;
  }
  .base:hover {
    cursor: pointer;
  }
  .disk {
    height: 15px;
    text-align: center;
    padding: 10px;
    margin-left: 25%;
    border-radius: 2px;
    margin-top: 3px;
  }
  .disk:hover {
    cursor: pointer;
    background: green;
    color: white;
  }
  .disk1 {
    margin-left: 70px;
  }

  .disk2 {
    margin-left: 45px;
  }
  .disk3 {
    margin-left: 20px;
  }

  ul {
    list-style: none;
  }
  a {
    cursor: pointer;
    color: white;
  }
  h4 {
    text-align: center;
    margin-right: 30px;
  }
  h3 {
    margin-top: 5px;
  }
  .moves {
    color: white;
  }
  .win {
    color: white;
  }
  .inst {
    font-size: 14px;
    margin: 50px;
    padding: 10px;
    color: white;
    border: 1px dotted white;
    margin-bottom: 50px;
    text-align: left;
  }
  .error {
    color: red;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
    opacity: 0
  }
</style>
