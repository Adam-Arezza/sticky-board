<template>
  <div v-if="warningDates" class="warnings">
    <strong class="toggler" @click="show = !show">Tasks past due:</strong>
    <span>{{warningDates}}</span>
    <div class="warningList" v-if="show">
      <ul>
        <li v-for="(sticky, index) in pastDue" :key="index">
          <strong>Column:</strong>
          {{sticky.column}}
          <br />
          <strong>Task:</strong>
          {{sticky.content}}
          <br />
          <strong>Days late:</strong>
          {{sticky.late}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: ["stickies"],
  data() {
    return {
      show: false,
      pastDue: []
    };
  },
  computed: {
    warningDates() {
      let warningCount = 0;
      this.stickies.forEach(sticky => {
        let today = new Date();
        let dueDate = new Date(sticky.due);
        if (dueDate < today) {
          let daysLate = (today - dueDate) / (1000 * 3600 * 24);
          sticky.late = Math.floor(daysLate);
          this.pastDue.push(sticky);
          warningCount++;
        }
      });
      return warningCount;
    }
  }
};
</script>

<style scoped>
.warnings {
  color: red;
}
.toggler:hover {
  cursor: pointer;
  border-bottom: solid 2px;
}
span {
  color: black;
}

.warningList {
  display: block;
  position: absolute;
  top: 75px;
  right: 50px;
  z-index: 1;
  background: rgb(224, 224, 222);
  padding: 15px;
  border: solid black 1px;
  border-radius: 15px;
  color: black;
}
ul {
  max-width: 250px;
  word-wrap: break-word;
}
li {
  border-bottom: 1px solid;
}
</style>