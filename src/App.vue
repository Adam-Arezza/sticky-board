<template>
  <div id="app">
    <h2>Sticky Board</h2>
    <div id="stickyBoard">
      <div class="container-fluid">
        <input id="noteInput" v-model="task" type="text" placeholder="write note">
        <input v-model="owner" type="text" placeholder="assign to">
        <b-button id="addBtn" size="md" @click="addSticky">Add Note</b-button>
      </div>
      <div id="board" class="row no-gutters">
        <div id="newNotes" class="col-sm-4">
          <h2>Tasks</h2>
          <sticky-note
            v-for="(note, index) in this.newNotes"
            :key="index"
            :task="note.task"
            :owner="note.owner"
            @deleteNote="removeSticky(index,note)"
            @moveNote="moveSticky(index,note)"
            @assign="assignOwner"
          ></sticky-note>
        </div>
        <div id="inProgress" class="col-sm-4">
          <h2>In Progress</h2>
          <sticky-note
            v-for="(note, index) in this.inProgress"
            :key="index"
            :task="note.task"
            :owner="note.owner"
            @deleteNote="removeSticky(index,note)"
            @moveNote="moveSticky(index,note)"
            @assign="assignOwner"
          ></sticky-note>
        </div>
        <div id="completed" class="col-sm-4">
          <h2>Completed</h2>
          <sticky-note
            v-for="(note, index) in this.complete"
            :key="index"
            :task="note.task"
            :owner="note.owner"
            @deleteNote="removeSticky(index,note)"
            @moveNote="moveSticky(index,note)"
            @assign="assignOwner"
          ></sticky-note>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import stickyNote from "./components/sticky-note.vue";

export default {
  components: { stickyNote },
  data() {
    return {
      task: "",
      owner: "",
      newNotes: [],
      inProgress: [],
      complete: []
    };
  },
  methods: {
    addSticky() {
      this.newNotes.push({ task: this.task, owner: this.owner });
      (this.task = ""), (this.owner = "");
    },
    removeSticky(index, note) {
      if (this.newNotes.includes(note)) {
        this.newNotes.splice(index, 1);
      }
      if (this.inProgress.includes(note)) {
        this.inProgress.splice(index, 1);
      }
      if (this.complete.includes(note)) {
        this.complete.splice(index, 1);
      }
    },
    moveSticky(index, note) {
      if (this.newNotes.includes(note)) {
        this.inProgress.push(note);
        this.newNotes.splice(index, 1);
        return;
      }
      if (this.inProgress.includes(note)) {
        this.complete.push(note);
        this.inProgress.splice(index, 1);
        return;
      }
    },
    assignOwner(noteProps) {
      console.log(noteProps);
      var allNotes = [this.newNotes, this.inProgress, this.complete];
      allNotes.forEach(stickyGroup => {
        stickyGroup.forEach(element => {
          if (element.task == noteProps[1]) {
            element.owner = noteProps[0];
          }
        });
      });
    }
  }
};
</script>

<style>
#noteInput {
  width: 75%;
}
#board {
  min-height: 100vh;
  text-align: center;
}
#addBtn {
  margin: 15px;
}
#newNotes,
#inProgress {
  border-right: 1px solid black;
}
h2{
  padding: 20px;
}
</style>
