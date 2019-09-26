<template>
  <div>
    <b-jumbotron>
      <template >
        <h3>{{currentQuestion.question}}</h3>  
      </template>

      <hr class="my-4" />

      <p v-for="(answer, index) in answers" :key="index">
        {{answer}}
      </p>

      <b-button variant="success" href="#">Submit</b-button>
      <b-button variant="primary" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  name: "QuestionBox",
  props: {
    currentQuestion: Object,
    next: Function
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }
};
</script>

<style lang="scss" scoped>
</style>