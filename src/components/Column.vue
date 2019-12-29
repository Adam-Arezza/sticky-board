<template>
  <div
    class="column"
    @dragover.prevent="dragOn"
    @dragleave.prevent="dragLeave"
    @drop.prevent="dropItem"
  >
    <h1>{{name}}</h1>
    <StickyNote
      v-for="(sticky, index) in stickies"
      :key="index"
      v-bind:note="sticky"
      v-on:delete="deleteNote"
    ></StickyNote>
  </div>
</template>

<script>
import StickyNote from "./sticky-note";

export default {
  components: { StickyNote },
  props: {
    name: String,
    stickies: Array
  },
  methods: {
    dropItem(e) {
      let data = e.dataTransfer.getData("sticky");
      if (e.target.className == "column") {
        e.target.style.background = "white";
        this.$emit("transfer", data, this.name);
      }
    },
    dragOn(e) {
      const column = e.target;
      if (column.className == "column") {
        column.style.background = "lightgrey";
      }
    },
    dragLeave(e) {
      const column = e.target;
      if (column.className == "column") {
        column.style.background = "white";
      }
    },
    deleteNote(id) {
      this.$emit("delete", id);
    }
  }
};
</script>

<style scoped>
.column {
  display: flex;
  flex-direction: column;
  width: 28%;
  margin: 15px;
  border: grey solid 2px;
  text-align: center;
  border-radius: 15px;
  align-items: center;
  overflow-y: scroll;
  background: white;
}
@media only screen and (max-width: 500px){
  .column{
    min-width: 90%;
    min-height: 30%;
  }
} 
/* width */
.column::-webkit-scrollbar {
  width: 15px;
  border-radius: 15px;
}

/* Track */
.column::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 15px;
  max-height: 30px;
  display: none;
}

/* Handle */
.column::-webkit-scrollbar-thumb {
  background: rgb(78, 119, 196);
  border-radius: 10px;
}

/* Handle on hover */
.column::-webkit-scrollbar-thumb:hover {
  background: #555;
  width: 15px;
}
</style>