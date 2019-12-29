<template>
  <div id="app">
    <Boards v-if="!selectedBoard" v-on:newBoard="newBoard" v-on:chooseBoard="loadBoard"></Boards>
    <InputHeader
      v-if="selectedBoard"
      v-on:createSticky="createSticky"
      v-bind:stickies="stickies"
      v-bind:columns="columns"
      v-on:backToBoards="backToBoards"
    ></InputHeader>
    <StickyBoard
      v-if="selectedBoard"
      v-bind:stickies="stickies"
      v-bind:columns="columns"
      v-on:transfer="moveSticky"
      v-on:delete="deleteNote"
    ></StickyBoard>
  </div>
</template>

<script>
import InputHeader from "./components/InputHeader";
import StickyBoard from "./components/StickyBoard";
import Boards from "./components/Boards";

export default {
  components: { InputHeader, StickyBoard, Boards },
  data() {
    return {
      stickies: [],
      columns: ["Tasks", "In progress", "Complete"],
      count: 0,
      selectedBoard: undefined
    };
  },
  methods: {
    createSticky(header, content, date, due) {
      this.stickies.push({
        stickyId: "S" + (this.count + 1),
        header: header,
        content: content,
        column: "Tasks",
        date: date,
        due: due
      });
      this.count++;
      this.saveStickies();
    },
    moveSticky(data, col) {
      let noteObj = JSON.parse(data);
      let moveNote = this.stickies.find(s => s.stickyId == noteObj.stickyId);
      let newCol = col;
      moveNote.column = newCol;
      this.saveStickies();
    },
    saveStickies() {
      if (!this.selectedBoard) {
        return console.log("no board");
      }
      let board = this.selectedBoard;
      let stickies = this.stickies;
      let data = {
        board: board,
        stickies: stickies
      };
      if (!localStorage.getItem("boardData")) {
        return localStorage.setItem("boardData", JSON.stringify([data]));
      }
      let savedData = JSON.parse(localStorage.getItem("boardData"));
      let boardExists = false;
      savedData.find(board => {
        if (board.board == this.selectedBoard) {
          board.stickies = this.stickies;
          boardExists = true;
        }
      });
      if (boardExists) {
        return localStorage.setItem("boardData", JSON.stringify(savedData));
      } else {
        savedData.push(data);
        return localStorage.setItem("boardData", JSON.stringify(savedData));
      }
    },
    deleteNote(id) {
      let note = this.stickies.findIndex(sticky => sticky.stickyId == id);
      this.stickies.splice(note, 1);
      this.saveStickies();
    },
    newBoard(board) {
      this.selectedBoard = board;
      this.stickies = [];
      this.saveStickies();
    },
    loadBoard(board) {
      this.stickies = board.stickies;
      this.selectedBoard = board.board;
      this.count = board.stickies.length;
    },
    backToBoards() {
      this.selectedBoard = undefined;
    }
  }
};
</script>

<style>
body {
  margin: 0px;

}
#app {
  /* min-height: 100vh; */
  min-height: 100vh;
  max-height: 100vh;
  background: rgb(200, 200, 200);
  font-family: Arial, Helvetica, sans-serif;
    background: linear-gradient(to top right,rgb(250,250,250), rgb(100, 150, 200));

}
</style>
