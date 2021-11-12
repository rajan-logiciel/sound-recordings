<template>
  <div id="app">
    <SoundRecordings msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
  import SoundRecordings from './components/SoundRecordings.vue'

  export default {
    name: 'App',
    components: {
      SoundRecordings
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
  .bordered {
    border: 0 !important;
  }
  .pr-1 {
      margin:0 !important;
      padding-bottom: 0!important;
    }
</style>

