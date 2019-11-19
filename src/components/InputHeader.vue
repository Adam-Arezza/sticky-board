<template>
  <div class="inputs">
    <div>
      <textarea class="stickyInput" placeholder="Enter task" v-model="task" />
      <button class="createBtn" @click="createSticky">Create</button>
    </div>
    <div>
      <input class="created-by" placeholder="created by" v-model="created"/>
    </div>
    <Progress v-bind:stickies="stickies" v-bind:columns="columns"></Progress>
  </div>
</template>

<script>
import Progress from './Progress'
export default {
  components: {Progress},
  props: ["stickies", "columns"],
  data() {
    return {
      task: "",
      created: ""
    };
  },
  methods: {
    createSticky() {
      if(!this.created) {
        return alert("please input the task creator")
      }
      let date = new Date()
      this.$emit("createSticky", this.task, this.created, date);
      this.task = ""
      this.created = ""
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
}
.stickyInput {
  max-height: 200px;
  max-width: 300px;
  min-height: 150px;
  min-width: 150px;
}
.columnInput {
  font-size: 1.2em;
  margin: 20px;
  justify-self: end;
}
.createBtn {
  background: darkblue;
  color: white;
  border: none;
  font-size: 1.2em;
  display: block;
}

.createBtn:hover {
  cursor: pointer;
}
.created-by {
  margin-left: 10px;
}
</style>