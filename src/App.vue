<template>
  <div id="app">
    <InputHeader v-on:createSticky="createSticky" v-bind:stickies="stickies" v-bind:columns="columns"></InputHeader>
    <StickyBoard v-bind:stickies="stickies" v-bind:columns="columns" v-on:transfer="moveSticky" v-on:delete="deleteNote"></StickyBoard>
  </div>
</template>

<script>
import InputHeader from "./components/InputHeader";
import StickyBoard from "./components/StickyBoard";

export default {
  components: { InputHeader, StickyBoard },
  data() {
    return {
      stickies: [],
      columns: ["Tasks", "In progress", "Complete"],
      count: 0
    };
  },
  methods: {
    createSticky(content, date, due) {
      this.stickies.push({
        stickyId: "S" + (this.count + 1),
        content: content,
        column: "Tasks",
        date: date,
        due: due
      });
      this.count++;
      this.saveStickies()
    },
    moveSticky(data, col) {
      let noteObj = JSON.parse(data);
      let moveNote = this.stickies.find(s => s.stickyId == noteObj.stickyId);
      let newCol = col;
      moveNote.column = newCol;
      this.saveStickies()
    },
    saveStickies() {
      localStorage.setItem("boardData", JSON.stringify(this.stickies))
    },
    loadStickies() {
      if(localStorage.getItem("boardData")) {
        let stickies = JSON.parse(localStorage.getItem("boardData"))
        this.stickies = stickies
      }
    },
    deleteNote(id) {
      let note = this.stickies.findIndex(sticky => sticky.stickyId == id);
      this.stickies.splice(note, 1);
      this.saveStickies()
    }
  },
  created() {
    this.loadStickies()
  }
};
</script>

<style>
body {
  margin: 0px;
}
#app {
  min-height: 100vh;
  background: rgb(200,200,200);
  font-family: Arial, Helvetica, sans-serif;
}
</style>
