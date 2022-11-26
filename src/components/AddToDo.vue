<template>
  <form @submit.prevent="saveTodo">
    Title: <input class="patchIn" type="text" v-model="title" /> 
    Description: <input class="patchIn" type="text" v-model="description" />
    <v-btn class="patchBtn" small color="cyan" type="submit">Save</v-btn>
  </form>
</template>

<script>
export default {
  data: () => ({
    title: "",
    description: "",
  }),
  methods: {
    saveTodo() {
      if (this.title.trim()) {
        const newTodo = {
          title: this.title,
          description: this.description,
          isCompleted: false,
        };
        this.$emit("add-todo", newTodo);
      }
    },
  },
};
</script>

<style scoped>
.patchIn {
  margin-top: 10px;
  border: 1px solid black;
}

.patchBtn {
  margin-left: 5px;
  margin-bottom: 5px;
}
</style>
