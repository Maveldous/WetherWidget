<template>
  <div id="app">
    <wetherPlugin />
  </div>
</template>

<script>
import wetherPlugin from "./components/wetherPlugin.vue";

export default {
  name: "App",
  components: {
    wetherPlugin
  }
};
</script>

<style lang="scss">

  #app{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 700px;
    height: 500px;
    margin: 100px auto;
    background: linear-gradient(#7998c6, #ecc5be);
  }

</style>
