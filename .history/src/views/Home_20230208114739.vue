<template>
  <div class="home">
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" type="text" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.completed">
        {{ todo.text }}
        <button @click.prevent="removeTodo(todo)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      newTodo: "",
      todos: [],
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length + 1,
        text: this.newTodo,
        completed: false
      });
      this.newTodo = "";
    },
    removeTodo(todo){
this.todos.forEach()
    }
  },
};
</script>
