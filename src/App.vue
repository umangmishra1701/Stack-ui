<template>
  <div id="app">
    <Stack/>
  </div>
</template>

<script>
import Stack from './components/Stack.vue'

export default {
  name: 'App',
  components: {
    Stack
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
}
</style>
