<template>
  <div>
    <label v-for="option in options" :key="option" class="option-label">
      <input type="radio" :value="option" v-model="selected" @change="emitAnswer" />
      {{ option }}
    </label>
  </div>
</template>

<script>
export default {
  props: ['options', 'selectedAnswer'],
  emits: ['answer-selected'],
  data() {
    return {
      selected: this.selectedAnswer || null
    };
  },
  watch: {
    selectedAnswer(newVal) {
      this.selected = newVal;
    }
  },
  methods: {
    emitAnswer() {
      this.$emit('answer-selected', this.selected);
    }
  }
};
</script>

<style scoped>
.option-label {
  display: block;
  margin-bottom: 10px;
  cursor: pointer;
  padding: 10px;
  border: 1px solid #e2e2e2;
  border-radius: 8px;
  transition: background-color 0.3s;
}

.option-label input[type="radio"] {
  margin-right: 5px;
}

.option-label:hover {
  background-color: #f0f0f0;
}

.option-label.selected {
  font-weight: bold;
  background-color: #e2e2e2;
}
</style>