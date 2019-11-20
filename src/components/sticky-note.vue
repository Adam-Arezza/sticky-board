<template>
  <div
    v-bind:id="note.stickyId"
    class="note"
    draggable="true"
    @dragstart="startDrag"
    @dragover.stop
  >
    <p>{{note.content}}</p>
    <p>created by: {{note.createdBy}}</p>
    <button @click="deleteNote(note.stickyId)">Delete</button>
    <p class="date">
      <strong>Created date:</strong>
      {{note.date}}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    note: Object
  },
  methods: {
    startDrag(e) {
      e.dataTransfer.setData("sticky", JSON.stringify(this.note));
    },
    deleteNote(id) {
      this.$emit("delete", id);
    }
  }
};
</script>

<style>
.note {
  background: lightgoldenrodyellow;
  min-width: 50%;
  max-width: 70%;
  /* max-height: 300px;
  min-height: 10%; */
  margin: 15px;
  box-shadow: 4px 4px 2px grey;
  text-align: left;
  padding: 10px;
}
.date {
  font-size: 0.75em;
}
</style>
