<template>
  <div id="app">
    <Header />

    <genero />
  </div>
</template>

<script>
import "./styles/global.css";

import Header from "./components/Header";

import Genero from "./components/genero.vue";

export default {
  name: "App",
  components: {
    Header,
    Genero,
  },
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
