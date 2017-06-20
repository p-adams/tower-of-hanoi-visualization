<template>
    <div class="flex-container">
        puzzle canvas {{msg}}
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
    background-color: lightgrey;
    height: 250px;
    width: 500px;
  }
</style>
