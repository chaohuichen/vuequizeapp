<template>
  <header>{{ title }}</header>
</template>

<script>
export default {
  data() {
    return {
      title: "Quiz Application",
    };
  },
};
</script>

<style scoped>
header {
  background: #8b0000;
  color: white;
  padding: 20px;
  /* position: absolute; */
  /* width: 100%; */
  font-weight: bold;
  position: fixed;
  top: 0;
  width: 100%;
}
</style>
