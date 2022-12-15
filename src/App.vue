<template>
  <div id="app">
    <DragBlock />
  </div>
</template>

<script>
import DragBlock from './components/dragBlock.vue';

export default {
  name: 'App',
  components: {
    DragBlock
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
