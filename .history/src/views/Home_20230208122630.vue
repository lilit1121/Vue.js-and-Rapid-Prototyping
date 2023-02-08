<template>

  <div>
    <h2>Todo List</h2>
    <input type="text" v-model="newTodo" @keyup.enter="addTodo">
    <ul>
      <li v-for="(todo, index) in todos" :key="index" @dragstart="dragStart(index)" @dragover="dragOver(index)" @drop="drop(index)">
        <input type="checkbox" v-model="todo.done"/> {{ todo.text }}
      </li>
    </ul>
  </div>
</template>
// JavaScript Code

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [
        { text: 'Learn Vue.js', done: false },
        { text: 'Build a Todo List', done: false }
      ]
    }
  },
  methods: {
    addTodo() {
      this.todos.push({
        text: this.newTodo,
        done: false
      });
      this.newTodo = '';
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
  }
}
</script>
// CSS Code

<style>
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  li {
    background-color: #f1f1f1;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
  }
  li.dragged {
    opacity: 0.5;
  }
</style>