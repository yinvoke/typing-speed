<template>
  <div id="app">
    <typingSpeed/>
  </div>
</template>

<script>
import typingSpeed from './components/typingSpeed.vue'

export default {
  name: 'app',
  components: {
    typingSpeed
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
