<template>
  <GamePage />
</template>

<script setup>
import GamePage from "./components/GamePage.vue";
// import { onMounted } from "vue";

// onMounted(async () => {
//   const response = await fetch("http://localhost:3000/users/", {});
//   console.log(response);
// });
</script>

<style lang="scss">
@font-face {
  font-family: "uni0553";
  src: url("./fonts/uni0553-webfont.woff") format("woff");
}

*,
*::before,
*::after {
  margin: 0;
  font-family: "uni0553", sans-serif;
  background: #242021;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: right;
  font-weight: 400;
  color: white;
}
</style>
