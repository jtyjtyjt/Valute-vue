<template>
  <div class="App">
    <div class="wrapper">
      <Daily/>
    </div>
  </div>
</template>

<script>
import Daily from '@/components/Daily.vue'

export default {
  name: 'App',
  components: {
    Daily
  }
}
</script>

<style>
html,body{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
html {
  margin-left: calc(100vw - 100%);
}
body{
  height: 100%;
  width: 100%;
  background: #ebfcfa;
}
.wrapper{
  margin: 0 auto;
  max-width: 1200px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
}
</style>
