<template>
  <div id="questionBox" v-bind:class="{ isCorrect: isCorrect, isWrong: isWrong,noAnswer: noAnswer}">
    <h3>{{ questionNumber + 1 }}. {{ questionTitle }}</h3>
    <!-- loop the answers -->
    <img :src="getImage(singleQuestion.questionImage)" v-show="singleQuestion.questionImage" />
    <div id="answers" v-for="(answer, key) in answerChoices" v-bind:key="key">
      <div
        id="question-choice"
        v-bind:class="{ correctChoice:singleQuestion.answerKey===key &&finalDisplayAnswer && isWrong }"
      >
        <input type="radio" :value="key" v-model="userChoice" v-on:click="onChoose(answer, key)" />
        <label>{{ answer }}</label>
      </div>
    </div>
    <span id="answerTag" v-bind:class="{green: isCorrect, red:isWrong}">{{correctText}}</span>
  </div>
</template>

<script>
export default {
  props: [
    "singleQuestion",
    "questionNumber",
    "submitNoAnswer",
    "finalDisplayAnswer",
  ],
  data() {
    return {
      answer: "",
      answerChoices: [],
      questionTitle: "",
      userChoice: "",
      isCorrect: false,
      isWrong: false,
      noAnswer: false,
      correctText: "",
    };
  },
  watch: {
    submitNoAnswer: function (currentVal, oldVal) {
      if (currentVal) {
        if (!this.singleQuestion.userAnswer) {
          this.noAnswer = true;
        }
      }
    },
    finalDisplayAnswer: function (currentVal, oldVal) {
      if (currentVal) {
        if (this.singleQuestion.userAnswer) {
          this.noAnswer = false;
          this.showAnswerAndStyle();
        }
      }
    },
  },
  methods: {
    onChoose: function (answer, key) {
      this.singleQuestion.userAnswer = key;
      this.$emit("onChoose", {
        answer,
        key,
        questionNumber: this.questionNumber,
      });
    },
    getImage: function (imagePath) {
      // the current image path is locally and need one more file level to access assets
      if (imagePath !== null) {
        return require("../" + imagePath);
      } else {
        return null;
      }
    },
    showAnswerAndStyle: function () {
      if (this.singleQuestion.userAnswer === this.singleQuestion.answerKey) {
        this.isCorrect = true;
        this.correctText = "Correct";
      } else if (
        this.singleQuestion.userAnswer !== this.singleQuestion.answerKey
      ) {
        this.isWrong = true;
        this.correctText = "Wrong";
      } else {
        this.isCorrect = false;
        this.isWrong = false;
        this.correctText = "";
      }
    },
  },
  created() {
    this.answerChoices = this.singleQuestion.answerChoices;
    this.questionTitle = this.singleQuestion.questionTitle;
  },
  updated() {},
};
</script>

<style scoped>
#answers {
  margin: 5px;
}
img {
  height: 20vh;
  width: 20vw;
}
#answerTag {
  text-align: left;
  align-self: flex-end;
  margin: 10px;
  margin-right: 45px;
  margin-bottom: 15px;
  height: 10px;
  width: 10px;
}
#questionBox {
  display: flex;
  flex-direction: column;
  background: white;
  /* padding: 2rem 2rem; */
  padding-top: 1rem;
  padding-left: 1rem;
  padding-right: 1rem;
  margin: 20px;
  width: 50vw;
  border-radius: 8px;
  box-shadow: 0px 3px 4px rgba(0, 0, 0, 0.15);
}
#questionBox h3 {
  margin-left: 10px;
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
.noAnswer {
  border-color: yellow;
  border-width: 3px;
  border-style: solid;
}
#question-choice {
  /* padding: 0rem 5rem 0rem 5rem; */
  padding-top: 5px;
  padding-bottom: 5px;
  width: 80%;
}
.correctChoice {
  color: white;
  background: #008000;
  border-radius: 10px;
}
@media screen and (min-width: 768px) {
  #questionBox {
    width: 60vw;
    min-width: 450px;
  }
}
@media only screen and (max-width: 768px) {
  /* For mobile phones: */
  #questionBox {
    align-items: center;
    width: 10vw;
    min-width: 300px;
  }
  img {
    height: 20vh;
    width: 40vw;
  }
}
</style>
