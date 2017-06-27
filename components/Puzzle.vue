<template>
  <div>
    <div class="title">
      <h3>Tower of Hanoi Visualization</h3>
      <p>Click <a href="#">here</a> to learn how it's played</p>
    </div>
    <div class="flex-container">
      <div class="flex-item">source</div>
      <div class="flex-item">auxiliary</div>
      <div class="flex-item">destination</div>
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
      [this.selectedDisk, this.fromPole] = [disk, pole]
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
    width: 600px;
  }
  .flex-item {
    background-color: white;
    width: 300px;
    height: 200px;
    margin: 30px;
  }
  .title {
    color: white;
    text-align: center;
  }

  a {
    color: white;
  }

</style>
