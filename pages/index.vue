<template>
  <section class="container">
    <div>
      <Puzzle></Puzzle>
    </div>
  </section>
</template>

<script>
import Puzzle from '~components/Puzzle.vue'

export default {
  components: {
    Puzzle
  }
}
</script>

<style>
.container
{
  border: 1px solid red;
  background: #333333;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.title
{
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  color: #35495e;
  letter-spacing: 1px;
}
.subtitle
{
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
.links
{
  padding-top: 15px;
}
</style>
