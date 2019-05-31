<template>
  <div ref="note" id="note" @dblclick="menu = !menu">
    <p>{{task}}</p>
    <p id="owner" v-if="owner">{{owner}}</p>
    <div id="menu" v-if="menu">
      <div v-if="!owner">
        <button @click="assign">Sign</button>
        <input v-model="newOwn" type="text" placeholder="assign to">
      </div>
      <b-button class="noteBtn" @click="deleteNote" size="sm">Delete sticky</b-button>
      <b-button class="noteBtn" @click="moveNote" size="md">></b-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: String,
    owner: String,
    colour: String
  },
  data() {
    return {
      menu: false,
      assigned: false,
      newOwn: ""
    };
  },
  methods: {
    deleteNote() {
      this.$emit("deleteNote");
    },
    moveNote() {
      this.$emit("moveNote");
    },
    assign() {
      this.$emit("assign", [this.newOwn, this.task]);
    }
  },
  mounted() {
    if (this.colour) {
      this.$refs.note.style.background = this.colour;
    }
  }
};
</script>

<style>
#note {
  background: lightgoldenrodyellow;
  max-width: 250px;
  max-height: 250px;
  min-height: 50px;
  margin: 15px;
  box-shadow: 4px 4px 2px grey;
  text-align: left;
}
p {
  padding: 10px;
}
#menu {
  padding: 10px;
}
#owner {
  font-size: 12px;
}
#message {
  padding: 10px;
}
.noteBtn {
  margin: 10px;
}
</style>
