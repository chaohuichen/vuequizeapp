<template>
  <app-layout>
    <div id="main-div">
      <!-- title -->
      <h1>{{title}}</h1>
      <!-- quiz result -->
      <app-quizResult
        :showResult="showResult"
        :resultPrecentage="resultPrecentage"
        :resultFraction="resultFraction"
      ></app-quizResult>
      <form id="main-form">
        <!-- loop the questions in json -->
        <div v-for="(question,index) in quizQuestions" v-bind:key="index">
          <app-singleQuestion
            :singleQuestion="question"
            :key="index"
            :questionNumber="index"
            :submitNoAnswer="submitNoAnswer"
            :finalDisplayAnswer="finalDisplayAnswer"
            v-on:onChoose="updateUserAnswer($event)"
          ></app-singleQuestion>
        </div>
      </form>
      <button type="submit" v-on:click="onSubmit">Submit</button>
      <!-- error span -->
      <span
        v-show="!AnswerdAllQuestion"
      >Answer all questions before submitting. Unanswered questions are displayed in yellow</span>
    </div>
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
    "app-singleQuestion": Singlequestion,
    "app-quizResult": QuizResult,
  },
  data() {
    return {
      title: "Quiz Application",
      quizQuestions: [],
      AnswerdAllQuestion: true,
      isSubmited: false,
      resultPrecentage: 0,
      resultFraction: "0 / 0",
      showResult: false,
      finalDisplayAnswer: false,
      submitNoAnswer: false,
    };
  },
  methods: {
    updateUserAnswer: function (data) {},
    validateAnswers: function () {
      this.AnswerdAllQuestion = true;
      this.quizQuestions.forEach((question) => {
        if (!question.hasOwnProperty("userAnswer")) {
          this.AnswerdAllQuestion = false;
        }
      });
      this.submitNoAnswer = !this.AnswerdAllQuestion && this.isSubmited;
    },

    calualteResult: function () {
      const length = this.quizQuestions.length;
      let correctCount = 0;

      this.quizQuestions.forEach((question) => {
        if (question.hasOwnProperty("userAnswer")) {
          if (question.userAnswer === question.answerKey) {
            question.isCorrect = true;
            correctCount++;
          }
        } else {
          this.AnswerdAllQuestion = false;
        }
      });
      this.resultFraction = `${correctCount} / ${length}`;
      this.resultPrecentage = (correctCount / length) * 100;
    },
    onSubmit: function () {
      if (!this.showResult) {
        this.isSubmited = true;
        this.validateAnswers();

        if (this.AnswerdAllQuestion) {
          this.calualteResult();
          this.finalDisplayAnswer = true;
          this.showResult = true;
        } else {
          this.isSubmited = false;
        }
      }
    },
  },

  created() {
    //reading the data from json
    const { myQuestions } = questions;
    this.quizQuestions = [...myQuestions.data];
    this.title = myQuestions.quizTitle;
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
#main-div h1 {
  margin-top: 5rem;
  margin-bottom: -2px;
}
#main-div span {
  color: red;
  margin: 10px;
}
</style>
