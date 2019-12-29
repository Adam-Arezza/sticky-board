<template>
  <div class="board-list">
    <div class="board-item" v-for="(board, index) in boards" :key="index">
      <div>
        <p>{{board.board.toUpperCase()}}</p>
        <button class="boardBtn" @click="chooseBoard(board)">Open</button>
        <button class="boardBtn" @click="deleteBoard(board)">Delete</button>
        <button class="boardBtn" @click="archiveBoard(board)">Archive</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["boards"],
  methods: {
    chooseBoard(board) {
      this.$emit("chooseBoard", board);
    },
    deleteBoard(board) {
      this.$emit("deleteBoard", board);
    },
    archiveBoard(board) {
      this.$emit("archiveBoard", board);
    }
  }
};
</script>

<style scoped>
.board-list {
  padding: 0px;
  margin: 0px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
p {
  margin: 0px;
  padding: 5px;
  border-radius: 5px;
  align-self: center;
  background: rgb(229, 231, 240);
  color: black;
}
.board-item {
  font-size: 1.2em;
  background: rgb(150, 154, 155);
  border: 2px solid black;
  margin: 10px;
  border-radius: 5px;
  width: 300px;
  min-height: 100px;
}
.board-item:hover {
  box-shadow: 4px 4px 2px grey;
}
.board-select:hover {
  cursor: pointer;
  background: rgb(197, 158, 235);
}
.boardBtn {
  border: none;
  margin: 5px;
  font-size: 1em;
  padding: 5px;
  border-radius: 20px;
  border: solid rgb(0, 30, 128) 2px;
}
.boardBtn:focus {
  outline: none;
}
.boardBtn:hover {
  cursor: pointer;
  background: rgb(178, 174, 245);
}
</style>