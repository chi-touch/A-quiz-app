<template>
  <div class="question-container">
    <h2 class="question-text">{{ question.text }}</h2>
    <MultipleChoice v-if="question.type === 'multiple-choice'" :options="question.options" @answer-selected="handleAnswer" :selectedAnswer="userAnswers[currentQuestionIndex]"/>
    <DragAndDrop v-if="question.type === 'drag-and-drop'" :matches="question.matches" @matches-submitted="handleAnswer" :userMatches="userAnswers[currentQuestionIndex]"/>
    <FeedbackComponent :message="feedbackMessage" :isCorrect="isCorrect"/>
    <button class="next-button" @click="nextQuestion">Next</button>
  </div>
</template>

<script>
import MultipleChoice from './MultipleChoice.vue';
import DragAndDrop from './DragAndDrop.vue';
import FeedbackComponent from './FeedbackComponent.vue';

export default {
  components: {
    MultipleChoice,
    DragAndDrop,
    FeedbackComponent
  },
  props: ['question', 'currentQuestionIndex', 'userAnswers'],
  emits: ['answer-selected', 'next-question'],
  data() {
    return {
      feedbackMessage: null,
      isCorrect: null
    }
  },
  methods: {
    handleAnswer(answer) {
      this.$emit('answer-selected', answer);
      if (this.question.type === 'multiple-choice') {
        if (answer === this.question.correctAnswer) {
          this.feedbackMessage = "Correct!";
          this.isCorrect = true;
        } else {
          this.feedbackMessage = "Incorrect. Try again.";
          this.isCorrect = false;
        }
      } else if (this.question.type === 'drag-and-drop') {
        let correct = true;
        for (const term in answer) {
          if (answer[term] !== this.question.matches[term]) {
            correct = false;
            break;
          }
        }
        if (correct) {
          this.feedbackMessage = "Correct!";
          this.isCorrect = true;
        } else {
          this.feedbackMessage = "Incorrect. Try again.";
          this.isCorrect = false;
        }
      }
    },
    nextQuestion() {
      this.$emit('next-question');
    }
  }
};
</script>

<style scoped>
.question-container {
  background-color: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.question-text {
  font-size: 1.4rem;
  margin-bottom: 20px;
  text-align: center;
}

.next-button {
  background-color: #5e5ce6;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
  margin-top: 20px;
}
</style>