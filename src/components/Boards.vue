<template>
  <div class="boards">
    <div class="newboard">
      <input class="boardname" placeholder="board name" type="text" v-model="boardName" />
      <button class="createBtn" @click="newBoard">Start new board</button>
    </div>
    <BoardList
      v-bind:boards="boards"
      v-on:chooseBoard="chooseBoard"
      v-on:deleteBoard="deleteBoard"
      v-on:archiveBoard="archiveBoard"
    ></BoardList>
  </div>
</template>

<script>
import BoardList from "./BoardList";
import { ipcRenderer } from "electron";

export default {
  components: { BoardList },
  data() {
    return {
      boardName: undefined,
      boards: []
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
      this.$emit("chooseBoard", board);
    },
    sendBoards() {
      ipcRenderer.send("boardList", this.boards);
    },
    deleteBoard(board) {
      let boards = JSON.parse(localStorage.getItem("boardData"))
      let filteredBoards = boards.filter(b => b.board != board.board)
      localStorage.setItem('boardData', JSON.stringify(filteredBoards))
      this.boards = filteredBoards
    },
    archiveBoard(board) {
      localStorage.setItem("archives", JSON.stringify(board))
      this.deleteBoard(board)
    }
  },
  mounted() {
    this.boards = JSON.parse(localStorage.getItem("boardData"));
    ipcRenderer.on("openBoard", (event, msg) => {
      let board = this.boards.find(board => board.board == msg);
      this.chooseBoard(board);
    });
  },
  watch: {
    boards: function(oldBoards, newBoards) {
      // console.log(oldBoards)
      // console.log(newBoards)
      if (oldBoards != newBoards) {
        console.log("sending boards")
        this.sendBoards();
      }
    }
  }
};
</script>

<style scoped>
.boards {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 20px;
}
.createBtn {
  background: rgb(50,50,100);
  color: white;
  border: none;
  font-size: 1.5em;
  padding: 15px;
  /* display: block; */
  margin: 3px;
  border-radius: 5px;
}
.createBtn:hover {
  cursor: pointer;
  background: rgb(114, 107, 247);
}
.boardname {
  font-size: 2em;
  margin: 5px;
  padding: 10px;
  width: 100%;
  border-radius: 5px;
  border: none;
  outline: none;
}
.newboard {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>