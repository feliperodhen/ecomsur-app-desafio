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

</style>

