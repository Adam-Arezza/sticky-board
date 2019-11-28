<template>
  <div class="inputs">
    <div>
      <textarea class="stickyInput" placeholder="Enter task" v-model="task" />
      <button class="actionBtn" @click="createSticky">Create</button>
    </div>
    <div class="details">
      <div>
        <label for="due=date">Due:</label>
        <input id="due-date" class="detail" type="date" v-model="dueDate" />
      </div>
      <Progress v-bind:stickies="stickies" v-bind:columns="columns"></Progress>
    </div>
    <button class="actionBtn" @click="backToBoards">Back to boards</button>
    <div class="date">{{currentDate}}
      <Warnings v-bind:stickies="stickies"></Warnings>
    </div>
  </div>
</template>

<script>
import Progress from "./Progress";
import Warnings from './Warnings'

export default {
  components: { Progress, Warnings },
  props: ["stickies", "columns"],
  data() {
    return {
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
      this.$emit("createSticky", this.task, formatDate, this.dueDate);
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
  padding: 15px;
}
.stickyInput {
  min-width: 300px;
  max-width: 400px;
  min-height: 50px;
  max-height: 175px;
}
.actionBtn {
  background: darkblue;
  color: white;
  border: none;
  font-size: 1.2em;
  display: block;
  border-radius: 5px;
}
.actionBtn:hover {
  cursor: pointer;
  background: rgb(50, 50, 100);
}
.details {
  display: flex;
  flex-direction: column;
  margin-left: 5px;
}
.detail {
  margin-left: 2px;
  margin-bottom: 3px;
  width: 150px;
}
.date {
  font-size: 1em;
}
</style>