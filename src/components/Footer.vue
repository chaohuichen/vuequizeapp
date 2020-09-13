<template>
  <footer>
    <h4>
      Created with
      <a href="https://vuejs.org/">Vue.js</a> & hosted on Netilify. Developed by
      <a href="https://www.chaohuichen.com/">Peter Chen</a>
    </h4>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      title: "Quiz App",
    };
  },
};
</script>

<style scoped>
footer {
  /* bottom: 0; */
  /* width: 100%; */
  padding: 20px;
}
h4 {
  text-align: center;
}
</style>
