<template>
  <div id="app">    
    <HelloWorld/>
    <!-- <div id="drawing"></div> -->
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
// import SVG from 'svg.js'

export default {
  name: 'App',
  mounted(){
    // // SVG.on(document, 'DOMContentLoaded', function() {
    //       // initialize SVG.js
    //   var draw = SVG('drawing')
    //   // draw pink square
    //   draw.rect(100, 100).move(100, 50).fill('#f06')
    // // })
  },
  components: {
    HelloWorld
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
  background-color: lightgrey;
}
</style>
