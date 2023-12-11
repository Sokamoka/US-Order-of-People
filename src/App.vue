<template>
  <main class="flex w-screen h-screen justify-center items-center">
    <Names />
  </main>
</template>

<script>
import Names from "./components/Names.vue";
export default {
  name: "App",
  components: {
    Names,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap");

#app {
  font-family: "Bebas Neue", cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
