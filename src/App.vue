<template>
  <div id="app">
    <img alt="Key logo" src="./assets/logo.png">
    <KeyPress/>
  </div>
</template>

<script>
import KeyPress from './components/KeyPress.vue';

export default {
  name: 'App',
  components: {
    KeyPress,
  },
};
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

img {
  height: 150px
}
</style>
