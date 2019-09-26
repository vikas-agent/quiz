<template>
  <div>
    <b-jumbotron>
      <template>
        <h3>{{currentQuestion.question}}</h3>
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
        v-for="(answer, index) in answers"
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedAnswer === index ? 'selected' : '']">
          {{answer}}
        </b-list-group-item>
       
      </b-list-group>

      <b-button variant="success" href="#">Submit</b-button>
      <b-button variant="primary" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  name: "QuestionBox",
  data() {
    return {
      selectedAnswer: null,
      shuffledAnswers: []
    }
  },
  props: {
    currentQuestion: Object,
    next: Function
  },
  methods: {
    selectAnswer(index) {
      this.selectedAnswer = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];

      this.shuffledAnswers = _.shuffle(answers);
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion() {
      this.selectedAnswer = null;
      this.shuffleAnswers();
    }
  }
};
</script>

<style lang="scss" scoped>
.list-group {
  padding-bottom : 1.5rem;
  
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