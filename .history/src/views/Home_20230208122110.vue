<template>
  <div class="home">
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" type="text" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li
        v-for="(todo, index) in todos"
        :key="todo.id"
        @dragstart="dragStart(index)"
        @dragover="dragOver(index)"
        @drop="drop(index)"
      >
        <input type="checkbox" v-model="todo.completed" />
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
        completed: false,
      });
      this.newTodo = "";
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((el) => el.id != todo.id);
    },
     dragStart(index) {
      this.draggedTodo = this.todos[index];
      this.draggedIndex = index;
    },
    dragOver(index) {
      const draggedOverTodo = this.todos[index];
      if (this.draggedTodo === draggedOverTodo) {
        return;
      }
      this.todos.splice(index, 0, this.draggedTodo);
      this.todos.splice(this.draggedIndex, 1);
      this.draggedIndex = index;
    },
    drop(index) {
      this.draggedTodo = null;
      this.draggedIndex = null;
    }
  },
};
</script>
<style></style>