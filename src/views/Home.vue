<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo,
  },
  data() {
    return {
      msg: "Hello",
      todos: [],
    };
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(this.todos = this.todos.filter((todo) => todo.id !== id))
      .catch(err => console.log(err));
    },
    addTodo(newTodo) {
      const {title, completed} = newTodo;
      axios.post("https://jsonplaceholder.typicode.com/todos", {title, completed})
      .then(res =>  this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));
    },
  },
  created() {
    console.log("Sending GET request...");
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then((res) => (this.todos = res.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

#app {
  max-width: 640px;
  margin: 0 auto;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
