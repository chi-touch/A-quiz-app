<template>
  <div id="app">
    <QuestionComponent :question="currentQuestion" :currentQuestionIndex="currentQuestionIndex" :userAnswers="userAnswers" @answer-selected="handleAnswer" @next-question="nextQuestion"/>
  </div>
</template>

<script>
import QuestionComponent from './components/QuestionComponent.vue';

export default {
  components: {
    QuestionComponent
  },
  data() {
    return {
      currentQuestionIndex: 0,
      questions: [
        {
          text: "What is Vue.js?",
          type: "multiple-choice",
          options: ["A JavaScript framework", "A CSS framework", "A database", "A text editor"],
          correctAnswer: "A JavaScript framework"
        },
        {
          text: "Match the following Vue.js concepts with their descriptions:",
          type: "drag-and-drop",
          matches: {
            "Component": "Reusable Vue instance",
            "Directive": "Special token in the markup",
            "Vue Router": "Library for routing",
            "Vuex": "State management pattern"
          }
        },
        {
          text: "Which of these is NOT a Vue.js directive?",
          type: "multiple-choice",
          options: ["v-if", "v-for", "v-else", "v-while"],
          correctAnswer: "v-while"
        },
        {
          text: "Match the following Vue.js lifecycle hooks with their descriptions:",
          type: "drag-and-drop",
          matches: {
            "created": "Called after the instance is created",
            "mounted": "Called after the instance is mounted",
            "updated": "Called after data changes",
            "destroyed": "Called after the instance is destroyed"
          }
        }
      ],
      score: 0,
      userAnswers: {}
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex];
    }
  },
  methods: {
    handleAnswer(answer) {
      this.userAnswers[this.currentQuestionIndex] = answer;
      if (this.currentQuestion.type === 'multiple-choice') {
        if (answer === this.currentQuestion.correctAnswer) {
          this.score++;
          alert("Correct!");
        } else {
          alert("Incorrect.");
        }
      } else if (this.currentQuestion.type === 'drag-and-drop') {
        let correct = true;
        for (const term in answer) {
          if (answer[term] !== this.currentQuestion.matches[term]) {
            correct = false;
            break;
          }
        }
        if (correct) {
          this.score++;
          alert("Correct!");
        } else {
          alert("Incorrect.");
        }
      }
    },
    nextQuestion() {
      this.currentQuestionIndex++;
      if (this.currentQuestionIndex >= this.questions.length) {
        alert("Quiz finished! Your score: " + this.score);
        this.currentQuestionIndex = 0;
        this.score = 0;
        this.userAnswers = {};
      }
    }
  }
};
</script>

<style scoped>
body {
  font-family: 'Inter', sans-serif;
  background-color: #f4f4f4;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  margin: 0;
}

#app {
  width: 90%;
  max-width: 375px;
  background-color: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}
</style>