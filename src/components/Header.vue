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
  background: red;
  color: white;
  padding: 20px;
  /* position: absolute; */
  /* width: 100%; */
}
</style>
