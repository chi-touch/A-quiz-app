<template>
  <div :class="feedbackClass">
    {{ message }}
  </div>
</template>

<script>
export default {
  props: ['message', 'isCorrect'],
  computed: {
    feedbackClass() {
      return this.isCorrect ? 'correct' : 'incorrect';
    }
  }
};
</script>

<style scoped>
.correct {
  color: green;
}
.correct,
.incorrect {
  font-size: 1.1rem;
  text-align: center;
  margin: 10px 0;
}
.incorrect {
  color: red;
}
</style>