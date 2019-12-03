<template>
  <div class="progress">
    <strong>{{percentComplete}} completed</strong>
    <div class="progress-bar" ref="p-bar">
      <div class="increment" v-for="(percent, index) in percent" :key="index">|</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["stickies", "columns"],
  data() {
    return {
      percent: 0
    };
  },
  computed: {
    percentComplete() {
      const completed = [];
      this.stickies.forEach(sticky => {
        if (sticky.column == this.columns[2]) {
          completed.push(sticky);
        }
      });
      let completeTasks = completed.length;
      let total = this.stickies.length;
      if (completeTasks < total && completed[0] != undefined) {
        //console.log("some amount is completed");
        const percentage = (completeTasks / total).toPrecision(2);
        this.percent = percentage * 100;
        return percentage * 100 + "%";
      }
      if (completeTasks == total && completed[0] != undefined) {
        const percentage = (completeTasks / total).toPrecision(2);
        this.percent = percentage * 100;
        return percentage * 100 + "%";
      } else {
        this.percent = 0;
        return 0 + "%";
      }
    }
  }
};
</script>

<style scoped>
.progress-bar {
  display: grid;
  grid-template-columns: repeat(100, 1fr);
  align-self: center;
  background: lightgray;
  border-radius: 10px;
  margin: 5px;
}
.increment {
  background: lightgreen;
}
</style>