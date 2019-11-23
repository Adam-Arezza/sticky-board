<template>
  <div class="board-list">
    <ul>
      <li v-for="(board, index) in boards" :key="index" @click="chooseBoard(board)">
        <p>{{board.board.toUpperCase()}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["boards"],
  methods: {
    chooseBoard(board) {
      this.$emit("chooseBoard", board);
    }
  }
};
</script>

<style scoped>
.board-list {
  margin: 20px;
  width: 30%;
}
.board-list ul {
  list-style: none;
  padding: 0px;
  margin: 0px;
}
li {
  display: flex;
  justify-content: center;
  font-size: 1.2em;
  background: rgb(207, 239, 247);
  border: 2px solid black;
  margin: 10px;
  padding: 5px;
  border-radius: 5px;
  height: 75px;
}
ul :hover {
  cursor: pointer;
  background: lightblue;
}
p {
  margin: 0px;
  padding: 0px;
  align-self: center;
}
</style>