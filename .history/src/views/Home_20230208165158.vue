<template>
  <div class="home">
    <h2>To do: ({{ todos.length }})</h2>
    <div class="flex">
      <form @submit.prevent="addTodo">
        <input
          v-model="newTodo"
          type="text"
          placeholder="Type something and press ENTER"
        />
      </form>
      <button class="mark_unmar" @click="markUnmar">
        {{ mark_unmar ? "Unmark" : "Mark" }}
      </button>
    </div>
    <ol>
      <ul>
        <li
          v-for="(todo, index) in todos"
          :key="todo.id"
          @dragstart="dragStart(index)"
          @dragover="dragOver(index)"
          @dragend="dragend(index)"
          draggable="true"
        >
          <label>
            <input type="checkbox" v-model="todo.completed" />
            <span>{{ todo.text }}</span>
            <a class="remove" @click.prevent="removeTodo(todo)">Remove</a>
          </label>
        </li>
      </ul>
    </ol>
    <hr />
    <h2>Completed items: ({{ completedItems }})</h2>
    <ol></ol>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      mark_unmar: false,
      newTodo: "",
      todos: [],
      draggedTodo: null,
      draggedIndex: null,
      completedItems: 0,
    };
  },
  methods: {
    markUnmar() {
      this.todos.sort((a, b) =>
        this.mark_unmar ? a.completed - b.completed : b.completed - a.completed
      );
      this.mark_unmar = !this.mark_unmar;
    },
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
  watch: {
    todos: {
      completed() {
        this.todos.forEach((el) =>
          el.completed ? ++this.completedItems : null
        );
      },
    },
  },
};
</script>
<style lang="scss" scoped>
h2 {
  font-weight: bold;
  margin-bottom: 15px;
}
input[type="text"] {
  padding: 10px;
  width: 200px;
}
label .remove {
  display: none;
  color: red;
}
label:hover .remove {
  display: inline-block;
  margin-left: 10px;
  color: red;
  opacity: 0.5;
}
button {
  cursor: pointer;
}
.flex {
  display: flex;
}
.mark_unmar {
  margin-left: 25px;
}
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
li {
  width: fit-content;
  background-color: #f1f1f1;
  padding: 10px;
  margin: 8px 0;
  border-radius: 5px;
}
li.dragged {
  opacity: 0.5;
}
</style>