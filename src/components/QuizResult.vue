<template>
  <div id="quiz-result">
    <h2 id="result-title">Result</h2>
    <h2>You got 5 / 10 questions correct</h2>
    <h2 v-bind:class="{red:true}">0%</h2>
  </div>
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
#quiz-result {
  /* display: flex;
  flex-direction: column;
  align-items: center;
  align-content: center; */
}
#result-title {
  text-decoration-line: underline;
}
h2 {
  text-align: center;
}
.red {
  color: red;
}
</style>
