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
      msg: 'woof'
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
