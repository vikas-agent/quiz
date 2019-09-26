<template>
  <div>
    <b-jumbotron>
      <template v-slot:header>Some Question?</template>

      <hr class="my-4" />

      <p>
       List of Answers
      </p>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  name: "QuestionBox"
};
</script>

<style lang="scss" scoped>
</style>