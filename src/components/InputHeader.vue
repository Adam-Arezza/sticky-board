<template>
  <div class="inputs">
    <div>
      <textarea class="stickyInput" placeholder="Enter task" v-model="task" />
      <button class="createBtn" @click="createSticky">Create</button>
    </div>
    <div class="details">
      <input class="detail" placeholder="created by" v-model="created" />
      <input class="detail" type="date" />
      <Progress v-bind:stickies="stickies" v-bind:columns="columns"></Progress>
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
      created: ""
    };
  },
  methods: {
    createSticky() {
      if (!this.created) {
        return alert("please input the task creator");
      }
      let date = new Date();
      this.$emit("createSticky", this.task, this.created, date);
      this.task = "";
      this.created = "";
    }
  }
};
</script>

<style scoped>
.inputs {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
  padding: 5px;
}
.stickyInput {
  max-height: 200px;
  max-width: 300px;
  min-height: 150px;
  min-width: 150px;
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
</style>