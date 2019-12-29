<template>
  <div
    v-bind:id="note.stickyId"
    class="note"
    draggable="true"
    @dragstart="startDrag"
    @dragover.stop
  >
    <h2>{{note.header}}</h2>
    <p v-for="(item, index) in note.content" class="content" :key="index">{{item}}</p>
    <button class="delete" @click="deleteNote(note.stickyId)">Delete</button>
    <p class="date">
      <strong>Created date:</strong>
      {{note.date}}
    </p>
    <p class="date">
      <strong>Due:</strong>
      {{note.due}}
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
  },
  beforeMount() {
    let items = this.note.content;
    try{
    items = items.split("\n");
    this.note.content = items;
    }
    catch(err){
      console.log(err)
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
  border: solid lightgray 1px;
}
.note:hover {
  cursor: grabbing;
}
.date {
  font-size: 0.75em;
}
.content {
  word-wrap: break-word;
}
.delete {
  border: none;
  margin: 5px;
  font-size: 1em;
  padding: 5px;
  border-radius: 20px;
  border: solid rgb(0, 30, 128) 2px;
}
.delete:hover {
  cursor: pointer;
  background: rgb(178, 174, 245);
}
</style>
