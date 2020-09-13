<template>
  <div id="questionBox" v-bind:class="{ isCorrect: isCorrect, isWrong: isWrong,noAnswer: noAnswer}">
    <h3>{{ questionNumber + 1 }}. {{ questionTitle }}</h3>
    <!-- loop the answers -->
    <img :src="getImage(singleQuestion.questionImage)" v-show="singleQuestion.questionImage" />
    <div id="answers" v-for="(answerChoice, key) in answerChoices" v-bind:key="key">
      <div
        id="question-choice"
        v-bind:class="{ correctChoice:singleQuestion.answerKey===key &&finalDisplayAnswer && isWrong }"
      >
        <input
          type="radio"
          :disabled="disableInput"
          :value="key"
          v-model="userChoice"
          v-on:click="onChoose(key)"
        />
        <label>{{ answerChoice }}</label>
      </div>
    </div>
    <span id="answerTag" v-bind:class="{green: isCorrect, red:isWrong}">{{correctText}}</span>
  </div>
</template>

<script>
const dataDefault = {
  answer: "",
  answerChoices: [],
  questionTitle: "",
  userChoice: "",
  isCorrect: false,
  isWrong: false,
  noAnswer: false,
  correctText: "",
  disableInput: false,
  resetSingleQuestion: false,
};
export default {
  props: {
    singleQuestion: {
      type: Object,
      required: true,
    },
    questionNumber: {
      type: Number,
      required: true,
    },
    submitNoAnswer: {
      type: Boolean,
      required: true,
    },
    finalDisplayAnswer: {
      type: Boolean,
      required: true,
    },
    reset: {
      type: Boolean,
    },
  },
  data() {
    return {
      ...dataDefault,
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
          this.disableInput = true;
          this.showAnswerAndStyle();
        }
      }
    },
    reset: function (currentVal, oldVal) {
      this.resetSingleQuestion = currentVal;
      if (this.resetSingleQuestion) {
        this.singleQuestionRest();
      }
    },
  },
  methods: {
    onChoose: function (userAnswerkey) {
      this.singleQuestion.userAnswer = userAnswerkey;
      this.$emit("onChoose", false);
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
    singleQuestionRest: function () {
      Object.keys(dataDefault).forEach((k) => {
        if (Object.prototype.hasOwnProperty.call(this.$data, k)) {
          this.$data[k] = dataDefault[k];
        }
      });
      this.answerChoices = this.singleQuestion.answerChoices;
      this.questionTitle = this.singleQuestion.questionTitle;
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
  align-self: start;
  width: 90%;
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
  display: flex;
  flex-direction: row;
  margin-left: 20px;
  padding-top: 5px;
  padding-bottom: 5px;
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
