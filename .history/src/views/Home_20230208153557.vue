<template>
  <div class="home">
    <div class="flex">
      <form @submit.prevent="addTodo">
        <input v-model="newTodo" type="text" />
        <button type="submit">Add</button>
      </form>
      <button class="mark_unmar" @click="markUnmar">{{markunmar}}</button>
    </div>
    <ul>
      <li
        v-for="(todo, index) in todos"
        :key="todo.id"
        @dragstart="dragStart(index)"
        @dragover="dragOver(index)"
        @dragend="dragend(index)"
        draggable="true"
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
      draggedTodo: null,
      draggedIndex: null,
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: Date.now(),
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
      } else {
        this.draggedIndex = index;
        return;
      }
    },
    dragend(index) {
      const draggedOverTodo = this.todos[this.draggedIndex];
      if (this.draggedIndex < index) {
        this.todos.splice(this.draggedIndex, 0, this.draggedTodo);
        this.todos.splice(index + 1, 1);
      } else if (this.draggedIndex > index) {
        this.todos.splice(index, 0, draggedOverTodo);
        this.todos.splice(this.draggedIndex + 1, 1);
      }

      this.draggedTodo = null;
      this.draggedIndex = null;
    },
  },
};
</script>
<style lang="scss" scoped>
.flex{
  display: flex;
}
.mark_unmar{
  margin-left: 25px;
}
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
li {
  width: fit-content;
  background-color: #f1f1f1;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}
li.dragged {
  opacity: 0.5;
}
</style>