<template>
  <div>
    <b-jumbotron>
      <template>
        <h3>{{currentQuestion.question}}</h3>
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="showAnswer(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        @click="submitAnswer"
        :disabled="selectedAnswer === null || justAnswered"
        variant="success"
      >Submit</b-button>
      <b-button @click="next" variant="primary">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "QuestionBox",
  data() {
    return {
      selectedAnswer: null,
      shuffledAnswers: [],
      correctAnswerIndex: null,
      justAnswered: false
    };
  },
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedAnswer = null;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedAnswer = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctAnswerIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
      this.justAnswered = false;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedAnswer === this.correctAnswerIndex) {
        isCorrect = true;
      }
      this.justAnswered = true;
      this.increment(isCorrect);
    },
    showAnswer(index) {
      let answerClass = "";

      if (!this.justAnswered && this.selectedAnswer === index) {
        answerClass = "selected";
      } else if (this.justAnswered && this.correctAnswerIndex === index) {
        answerClass = "correct";
      } else if (
        this.justAnswered &&
        this.selectedAnswer === index &&
        this.correctAnswerIndex !== index
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    }
  }
};
</script>

<style lang="scss" scoped>
.list-group {
  padding-bottom: 1.5rem;

  .list-group-item:hover {
    background-color: gray;
    cursor: pointer;
  }

  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: red;
  }
}
.btn {
  margin: 0 5px;
}
</style>