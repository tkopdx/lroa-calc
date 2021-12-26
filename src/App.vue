<template>
  <Main/>
</template>

<script>
import Main from './components/Main.vue';

export default {
  name: 'App',
  components: {
    Main
  }
}
</script>

<style>
html {
  background-color: #2EA879;
  background: url('https://cdnb.artstation.com/p/assets/images/images/031/261/071/large/ondrej-hrdina-krabice12-3.jpg?1603111592') center center fixed;
  background-size: cover;
  scroll-behavior: smooth;
  transition: all 0.2s;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin: 0;
  backdrop-filter: blur(10px);
  min-height: 100vh;
  width: auto;
  min-width: 95vw;
  height: auto;
}

body {
  margin: 0;
}
</style>
