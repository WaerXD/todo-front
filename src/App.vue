<template>
  <v-app>
    <div id="app">
      <h1>ToDo App</h1>
      <v-btn id="btn1" color="primary" rounded>Домашняя Работа</v-btn>
      <br />
      <hr />

      <v-btn color="success" class="list-control" @click="getTodos()"
        >Получить</v-btn
      >
      <v-btn color="yellow" class="list-control" @click="isClicked = !isClicked"
        >Создать</v-btn
      >
      <v-btn color="error" class="list-control" @click="deleteTodos()">
        Удалить все
      </v-btn>
      <br />
      <addTodo v-if="isClicked" @add-todo="addTodo" />
      <h2>ToDo List:</h2>
      <TodoList
        v-bind:todos="todoList"
        @delete-todo="deleteTodo"
        @patch-todo="patchTodo"
      />
    </div>
  </v-app>
</template>

<script>
const axios = require("axios");
import TodoList from "@/components/ToDoList.vue";
import addTodo from "@/components/AddToDo.vue";
export default {
  name: "App",
  components: {
    TodoList,
    addTodo,
  },

  data: () => ({
    todoList: [],
    isClicked: false,
  }),

  methods: {
    async getTodos() {
      const todos = await axios.get("http://localhost:3000/items");
      this.todoList = todos.data.todoList;
    },

    async deleteTodos() {
      await axios.delete("http://localhost:3000/items");
      this.getTodos();
    },

    async addTodo(todo) {
      await axios.post("http://localhost:3000/items", {
        title: todo.title,
        description: todo.description,
        isCompleted: todo.isCompleted,
      });
      this.getTodos();
    },
    async deleteTodo(id) {
      await axios.delete("http://localhost:3000/items/" + id);
      this.getTodos();
    },
    async patchTodo(id) {
      const newTitle = prompt("Enter New Title");
      const newDescription = prompt("Enter New Description");
      if (newTitle && newDescription) {
        await axios.patch("http://localhost:3000/items/" + id, {
          title: newTitle,
          description: newDescription,
        });
        this.getTodos();
      }
    },
  },
};
</script>

<style>
.list-control {
  margin-top: 10px;
  margin-left: 15px;
}
#btn1 {
  margin-bottom: 10px;
}
#app {
  font-family: "Avenir", Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothig: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
</style>
