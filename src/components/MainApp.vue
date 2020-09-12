<template>
  <app-layout>
    <div id="main-div">
      <!-- title -->
      <h1>Quiz 1 - HTML / CSS / JS Practice</h1>
      <app-quizresult></app-quizresult>
      <form id="main-form">
        <!-- loop the questions in json -->
        <div v-for="(question,index) in quizQuestions" v-bind:key="index">
          <app-singlequestion
            :singleQuestion="question"
            :key="index"
            :questionNumber="index"
            :isSubmited="isSubmited"
            v-on:onChoose="updateUserAnswer($event)"
          ></app-singlequestion>
        </div>
      </form>

      <button id="submitButton" type="submit" v-on:click="onSubmit">Submit</button>
      <span
        v-show="!AnswerdAllQuestion"
      >Answer all questions before submitting. Unanswered questions are displayed in yellow</span>
    </div>
    <p>{{userAnswers}}</p>
    <p>{{quizQuestions}}</p>
    <p>{{answerkeys}}</p>
  </app-layout>
</template>

<script>
import Layout from "./Layout";
import questions from "../questions.json";
import Singlequestion from "./SingleQuestion";
import QuizResult from "./QuizResult";
export default {
  components: {
    "app-layout": Layout,
    "app-singlequestion": Singlequestion,
    "app-quizresult": QuizResult,
  },
  data() {
    return {
      title: "Quiz App",
      quizQuestions: [],
      userAnswers: {},
      answerkeys: [],
      AnswerdAllQuestion: true,
      isSubmited: false,
    };
  },
  methods: {
    updateUserAnswer: function (data) {
      const { questionNumber } = data;
      if (questionNumber in this.userAnswers) {
        this.userAnswers[`${questionNumber}`] = data;
      }
      console.log(this.userAnswers);
    },
    validateAnswers: function () {
      for (const answer in this.userAnswers) {
        if (answer in this.userAnswers) {
          if (this.userAnswers[answer] === null) {
            this.AnswerdAllQuestion = false;
          }
        }
      }
      console.log(this.AnswerdAllQuestion);
    },
    onSubmit: function () {
      this.validateAnswers();
    },
  },
  //   computed: {
  //     onChoose: function () {},
  //   },
  created() {
    //reading the data from json
    const { myQuestions } = questions;
    this.quizQuestions = myQuestions;

    //getting the answerKeys and setup user answers
    this.quizQuestions.forEach((question) => {
      this.answerkeys.push(parseInt(question.answerKey));
      this.userAnswers[question.id] = null;
    });
  },
};
</script>

<style scoped>
#main-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: transparent;
}
button {
  align-self: center;
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border-radius: 5px;
}
#main-div {
  display: flex;
  flex-direction: column;
  align-items: center;
}
#main-div span {
  color: red;
  margin: 10px;
}
/* .isCorrect {
  border-color: coral;
  border-width: thin medium thick 10px;
  background: red;
} */
</style>
