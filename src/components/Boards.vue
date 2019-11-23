<template>
  <div class="boards">
    <div class="newboard">
      <input class="boardname" placeholder="board name" type="text" v-model="boardName" />
      <button class="createBtn" @click="newBoard">Start new board</button>
    </div>
    <BoardList v-bind:boards="boards" v-on:chooseBoard="chooseBoard"></BoardList>
  </div>
</template>

<script>
import BoardList from "./BoardList";

export default {
  components: { BoardList },
  props: ["boards"],
  data() {
    return {
      boardName: undefined
    };
  },
  methods: {
    newBoard() {
      if (this.boardName) {
        this.$emit("newBoard", this.boardName);
      } else {
        alert("new board needs a name");
      }
    },
    chooseBoard(board) {
        this.$emit('chooseBoard', board)
    }
  }
};
</script>

<style scoped>
.boards {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
}
.createBtn {
  background: darkblue;
  color: white;
  border: none;
  font-size: 1.2em;
  display: block;
  margin: 3px;
  border-radius: 5px;
}
.createBtn:hover {
  cursor: pointer;
  background: rgb(50, 50, 100);
}
.boardname {
  font-size: 2em;
  margin: 5px;
  width: 100%;
}
.newboard {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>