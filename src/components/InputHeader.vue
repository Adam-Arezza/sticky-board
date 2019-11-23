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
    <div class="date" v-bind="currentDate">
      {{currentDate}}
    </div>
  </div>
</template>

<script>
import Progress from "./Progress";
export default {
  components: { Progress },
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
      this.$emit("createSticky", this.task, date, this.dueDate);
      this.task = "";
      this.created = "";
      this.dueDate = undefined;
    },
    backToBoards() {
      this.$emit("backToBoards")
    }
  },
  computed: {
    currentDate() {
      return new Date()
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
.createBtn:hover {
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