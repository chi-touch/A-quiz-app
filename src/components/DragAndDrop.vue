<template>
  <div class="drag-and-drop">
    <div v-for="(definition, term) in matches" :key="term" class="draggable-term" draggable="true" @dragstart="dragStart(term)">
      {{ term }}
    </div>
    <div class="drop-area" @dragover="dragOver" @drop="drop">
      <div v-for="(droppedTerm, droppedDefinition) in droppedMatches" :key="droppedTerm">
        {{ droppedTerm }} - {{ droppedDefinition }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['matches', 'userMatches'],
  emits: ['matches-submitted'],
  data() {
    return {
      droppedMatches: this.userMatches || {}
    };
  },
  methods: {
    dragStart(term) {
      event.dataTransfer.setData('term', term);
    },
    dragOver(event) {
      event.preventDefault();
    },
    drop(event) {
      event.preventDefault();
      const term = event.dataTransfer.getData('term');
      const definition = this.matches[term];
      this.droppedMatches[term] = definition;
      this.$emit('matches-submitted', this.droppedMatches);
    }
  }
};
</script>

<style scoped>
.draggable-term {
  background-color: #e2e2e2;
  padding: 12px;
  border-radius: 8px;
  margin-bottom: 8px;
  cursor: grab;
}

.drop-area {
  border: 2px dashed #5e5ce6;
  background-color: #f9f9f9;
  padding: 15px;
  min-height: 100px;
  border-radius: 7px;

}
</style>