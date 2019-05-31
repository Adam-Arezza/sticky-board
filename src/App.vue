<template>
  <div id="app">
    <h2>Sticky Board</h2>
    <div id="stickyBoard">
      <div class="container-fluid">
        <div class="row align-items-start">
          <div class="col-md-2">
            <select v-model="selectedColour">
              <option v-for="(colour, index) in colours" :key="index">{{colour}}</option>
            </select>
          </div>
          <div class="col-md-8">
            <input id="noteInput" v-model="task" type="text" placeholder="write note">
          </div>
          <div class="col-md-2">
            <b-button id="addBtn" size="md" @click="addSticky">Add Note</b-button>
          </div>
        </div>
        <div class="row">
          <div class="owner col-md-3 offset-md-2">
            <input id="ownerName" v-model="owner" type="text" placeholder="assign to">
          </div>
        </div>
      </div>
      <div id="board" class="row no-gutters">
        <div id="newNotes" class="col-sm-4">
          <h2>Tasks</h2>
          <sticky-note
            v-for="(note, index) in this.newNotes"
            :key="index"
            :task="note.task"
            :owner="note.owner"
            :colour="note.colour"
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
            :colour="note.colour"
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
            :colour="note.colour"
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
      complete: [],
      selectedColour: "",
      colours: ["red", "lightblue", "lightgreen", "yellow", "purple"]
    };
  },
  methods: {
    addSticky() {
      if (!this.task) {
        return alert("First add a note");
      }
      this.newNotes.push({ task: this.task, owner: this.owner , colour: this.selectedColour});
      (this.task = ""), (this.owner = ""), (this.selectedColour = "");
      this.saveStickys();
    },
    removeSticky(index, note) {
      if (this.newNotes.includes(note)) {
        this.newNotes.splice(index, 1);
        this.saveStickys();
      }
      if (this.inProgress.includes(note)) {
        this.inProgress.splice(index, 1);
        this.saveStickys();
      }
      if (this.complete.includes(note)) {
        this.complete.splice(index, 1);
        this.saveStickys();
      }
    },
    moveSticky(index, note) {
      if (this.newNotes.includes(note)) {
        this.inProgress.push(note);
        this.newNotes.splice(index, 1);
        this.saveStickys();
        return;
      }
      if (this.inProgress.includes(note)) {
        this.complete.push(note);
        this.inProgress.splice(index, 1);
        this.saveStickys();
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
      this.saveStickys();
    },
    saveStickys() {
      var stickyData = {
        newNotes: this.newNotes,
        inProgress: this.inProgress,
        complete: this.complete
      };
      localStorage.setItem("stickys", JSON.stringify(stickyData));
    }
  },
  created: function() {
    if (localStorage.getItem("stickys")) {
      var stickys = JSON.parse(localStorage.getItem("stickys"));
      this.newNotes = stickys.newNotes;
      this.inProgress = stickys.inProgress;
      this.complete = stickys.complete;
    }
  }
};
</script>

<style>
#noteInput {
  width: 100%;
}
#board {
  min-height: 100vh;
  text-align: center;
}

#newNotes,
#inProgress {
  border-right: 1px solid black;
}
h2 {
  padding: 20px;
}
select {
  width: 100%;
}
</style>
