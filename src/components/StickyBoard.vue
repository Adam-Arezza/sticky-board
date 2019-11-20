<template>
  <div class="board">
    <Column
      v-for="(column, index) in columns"
      :key="index"
      v-bind:name="column"
      v-bind:stickies="sortStickies(column)"
      v-on:delete="deleteNote"
      v-on:transfer="transferSticky"
    ></Column>
  </div>
</template>

<script>
import Column from "./Column";

export default {
  components: { Column },
  props: {
    stickies: Array,
    columns: Array
  },
  methods: {
    sortStickies(col) {
      let notes = [];
      this.stickies.forEach(sticky => {
        if (col == sticky.column) {
          notes.push(sticky);
        }
      });
      return notes;
    },
    deleteNote(id) {
      let note = this.stickies.findIndex(sticky => sticky.stickyId == id);
      this.stickies.splice(note, 1);
    },
    transferSticky(data, col) {
      this.$emit('transfer', data, col)
    }
  }
};
</script>

<style scoped>
.board {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  flex-wrap: wrap;
}
</style>