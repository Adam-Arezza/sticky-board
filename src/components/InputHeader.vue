<template>
  <div class="inputs">
    <div class="noteContents">
      <input class="stickyHeader" placeholder="Task Header" v-model="taskHead" />
      <textarea class="stickyInput" placeholder="Enter task" v-model="task" />
    </div>
    <div class="details">
      <div>
        <label class="date-label" for="due-date">Due:</label>
        <input id="due-date" class="detail" type="date" v-model="dueDate" />
        <button class="actionBtn" @click="createSticky">Create Note</button>
        <Progress v-bind:stickies="stickies" v-bind:columns="columns"></Progress>
      </div>
      <div>
        <button class="actionBtn" @click="backToBoards">Back to boards</button>
      </div>
    </div>
    <div class="date">
      {{currentDate}}
      <Warnings v-bind:stickies="stickies"></Warnings>
    </div>
  </div>
</template>

<script>
import Progress from "./Progress";
import Warnings from "./Warnings";

export default {
  components: { Progress, Warnings },
  props: ["stickies", "columns"],
  data() {
    return {
      taskHead: "",
      task: "",
      dueDate: undefined
    };
  },
  methods: {
    createSticky() {
      if (!this.dueDate) {
        return alert("please input the task due date");
      }
      let date = new Date();
      let month = date.getMonth();
      date = date
        .toString()
        .split(" ")
        .slice(1, 4);
      let formatDate = [date[2], month, date[1]].join("-");
      this.$emit(
        "createSticky",
        this.taskHead,
        this.task,
        formatDate,
        this.dueDate
      );
      this.taskHead = "";
      this.task = "";
      this.created = "";
      this.dueDate = undefined;
    },
    backToBoards() {
      this.$emit("backToBoards");
    }
  },
  computed: {
    currentDate() {
      let date = new Date().toString();
      let formatDate = date.split(" ").slice(0, 4);
      // console.log(formatDate)
      formatDate = formatDate.join(" ");
      return formatDate;
    }
  }
};
</script>

<style scoped>
.inputs {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: flex-start;
  padding: 20px;
  flex-wrap: wrap;
  max-height: 25vh;
}

@media only screen and (max-width: 500px) {
  .inputs {
    flex-direction: column;
  }
}

.stickyInput {
  width: 350px;
  height: 175px;
  resize: none;
  overflow-y: scroll;
}
.actionBtn {
  border: none;
  margin: 5px;
  font-size: 1em;
  padding: 5px;
  border-radius: 20px;
  border: solid rgb(0, 30, 128) 2px;
}
.actionBtn:hover {
  cursor: pointer;
  background: rgb(178, 174, 245);
}
.details {
  display: flex;
  flex-direction: column;
  margin-left: 5px;
}
.detail {
  margin-left: 2px;
  margin-bottom: 3px;
  font-size: 1.5em;
  border-radius: 10px;
}
.date {
  font-size: 1em;
}
.date-label {
  font-size: 1.3em;
}
.noteContents {
  display: flex;
  flex-direction: column;
}

@media only screen and (max-width: 500px) {
  .stickyInput {
    width: 100%;
  }
}
</style>