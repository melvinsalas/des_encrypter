<template>
  <div id="app">
    <des />
    <info />
    <copyright />
  </div>
</template>

<script>
import des from "./components/des"
import info from "./components/info"
import copyright from "./components/copyright"

export default {
  name: "App",
  components: {
    des,
    info,
    copyright,
  },
}
</script>

<style>
body {
  background-color: rgb(237, 242, 247);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
