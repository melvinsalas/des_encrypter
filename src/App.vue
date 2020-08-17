<template>
  <div id="app">
    <des msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import des from "./components/des.vue"

export default {
  name: "App",
  components: {
    des,
  },
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
