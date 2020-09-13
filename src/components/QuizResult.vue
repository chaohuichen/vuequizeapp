<template>
  <div id="quiz-result" v-show="showResult">
    <h2 id="result-title">Result</h2>
    <h2>You got {{resultFraction}} questions correct</h2>
    <h2 v-bind:class="{red:true}">{{resultPrecentage}}%</h2>
  </div>
</template>

<script>
export default {
  props: ["showResult", "resultPrecentage", "resultFraction"],
  data() {
    return {};
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
