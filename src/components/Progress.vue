<template>
  <div class="progress">{{percentComplete}}</div>
</template>

<script>
export default {
  props: ["stickies", "columns"],
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
        console.log("some amount is completed");
        const percentage = (completeTasks / total).toPrecision(2);
        return percentage * 100 + "%";
      }
      if (completeTasks == total && completed[0] != undefined) {
        const percentage = (completeTasks / total).toPrecision(2);
        return percentage * 100 + "%";
      } else {
        return 0 + "%";
      }
    }
  }
};
</script>

<style scoped>
</style>