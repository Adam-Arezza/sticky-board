<template>
  <div id="app">
    <InputHeader v-on:createSticky="createSticky" v-bind:stickies="stickies" v-bind:columns="columns"></InputHeader>
    <StickyBoard v-bind:stickies="stickies" v-bind:columns="columns" v-on:transfer="moveSticky"></StickyBoard>
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
    createSticky(content, created, date) {
      this.stickies.push({
        stickyId: "S" + (this.count + 1),
        content: content,
        column: "Tasks",
        createdBy: created,
        date: date
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
</style>
