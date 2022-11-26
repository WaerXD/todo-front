<template>
  <li>
    <span v-bind:class="{ complete: todo.isCompleted }">
      <input
        type="checkbox"
        @change="
          completionChange();
          todo.isCompleted = !todo.isCompleted;
        "
      />
      {{ todo.title }}. Описание: {{ todo.description }}
      <v-btn color="#f0f165" x-small fab @click="$emit('patch-todo', todo.id)">
        <v-icon> mdi-pencil </v-icon>
      </v-btn>

      <v-btn color="#e94b4b" x-small fab @click="$emit('delete-todo', todo.id)">
        <v-icon> mdi-delete </v-icon>
      </v-btn>
    </span>
  </li>
</template>

<script>
import axios from "axios";

export default {
  props: ["todo"],
  methods: {
    async completionChange() {
      await axios.patch("http://localhost:3000/items/" + this.$props.todo.id, {
        isCompleted: !this.$props.todo.isCompleted,
      });
    },
  },
};
</script>

<style scoped>
li {
  border: 2px solid black;
  display: flex;
  justify-content: space-between;
  padding: 0.5rem 2rem;
  margin-bottom: 1rem;
}
.complete {
  text-decoration: line-through;
}
input {
  margin-right: 1rem;
}
</style>
