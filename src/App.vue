<template>
  <div>
    <main-app></main-app>
  </div>
</template>

<script>
import MainApp from "./components/MainApp";

export default {
  components: {
    "main-app": MainApp,
  },
  data() {
    return {
      msg: "Vue Quiz App",
    };
  },
};
</script>

<style>
body {
  margin: 0;
  background: #f2f2f7;
}
</style>
