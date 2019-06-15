<template>
  <div id="app">
    <Player></Player>
  </div>
</template>

<script>
import Player from './components/Player'
import './assets/common.css'
import './assets/style.css'


export default {
  name: 'App',
  components: {
    Player
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
