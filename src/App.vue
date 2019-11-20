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
    },
    moveSticky(data, col) {
      let noteObj = JSON.parse(data);
      let moveNote = this.stickies.find(s => s.stickyId == noteObj.stickyId);
      let newCol = col;
      moveNote.column = newCol;
    }
  }
};
</script>

<style>
body {
  margin: 0px;
}
#app {
  background: rgb(200,200,200);
  font-family: Arial, Helvetica, sans-serif;
}
</style>
