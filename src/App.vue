<template>
  <div id="app">
    <Comics />
  </div>
</template>

<script>
import Comics from "./components/Comics.vue";

export default {
  name: "App",
  components: {
    Comics,
  },
};
</script>

<style lang="scss">
@import "./styles/style.scss";
body {
  background-image: url("https://images6.alphacoders.com/389/389726.jpg");
  background-repeat: repeat;
}
</style>

