<template>
  <div id="questionBox" v-bind:class="{isCorrect:isCorrect,isWrong:isWrong}">
    <h3>{{questionNumber+1}}. {{questionTitle}}</h3>
    <!-- loop the answers -->
    <div id="answers" v-for="(answer,key) in answerChoices" v-bind:key="key">
      <div id="question-choice" v-bind:class="{correctChoice:false}">
        <input type="radio" :value="key" v-model="userChoice" v-on:click="onChoose(answer,key)" />
        <label>{{answer}}</label>
      </div>
    </div>
    <span id="answerTag">Correct</span>
  </div>
</template>

<script>
export default {
  props: ["singleQuestion", "questionNumber", "isSubmited"],
  data() {
    return {
      answer: "",
      answerChoices: [],
      questionTitle: "",
      userChoice: "",
      isCorrect: true,
      isWrong: false,
    };
  },
  methods: {
    onChoose: function (answer, key) {
      this.$emit("onChoose", {
        answer,
        key,
        questionNumber: this.questionNumber,
      });
    },
  },
  created() {
    this.answerChoices = this.singleQuestion.answerChoices;
    this.questionTitle = this.singleQuestion.questionTitle;
  },
};
</script>

<style scoped>
#answers {
  margin: 5px;
}
#answerTag {
  color: red;
  text-align: left;
  align-self: flex-end;
  margin: 10px;
}
#questionBox {
  display: flex;
  flex-direction: column;
  background: white;
  /* padding: 2rem 2rem 2rem 2rem; */
  margin: 10px;
  width: 70rem;
  border-radius: 8px;
  box-shadow: 0px 3px 4px rgba(0, 0, 0, 0.15);
}
.isCorrect {
  border-color: green;
  border-width: 3px;
  border-style: solid;
}
.isWrong {
  border-color: red;
  border-width: 3px;
  border-style: solid;
}
#question-choice {
  /* padding: 0rem 5rem 0rem 5rem; */
}
.correctChoice {
  color: white;
  background: #008000;
  border-radius: 10px;
}
</style>
