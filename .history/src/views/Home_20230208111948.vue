<template>
  <div class="home">
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" type="text" />
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.text }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Home",
};
</script>
