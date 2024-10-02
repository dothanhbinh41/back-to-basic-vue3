<template>
  <div>
    <h1>Languages developer</h1>
    <div style="display: flex;">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Title...">
      <button @click="addTodo">Add</button>
    </div>

    <ul>
      <li v-for="(todo, index) in todos" :key="index" :class="{ completed: todo.completed }">
        <span @click="toggleComplete(todo)">{{ todo.text }}</span>
        <button @click="removeTodo(index)">×</button>
      </li>
    </ul>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    const newTodo = ref('');
    const todos = ref([
      {text: 'Laravel', completed: false},
      {text: 'Vue', completed: false},
      {text: 'React', completed: false},
      {text: 'C#', completed: false},
      {text: 'Python', completed: false},
      {text: 'GoLang', completed: false}
    ]);

    function addTodo() {
      if (newTodo.value.trim() === '') return;
      todos.value.push({text: newTodo.value, completed: false});
      newTodo.value = '';
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
    }

    function toggleComplete(todo) {
      todo.completed = !todo.completed;
    }

    return {
      newTodo,
      todos,
      addTodo,
      removeTodo,
      toggleComplete
    }
  },

  mounted() {
  }
}

</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 400px;
  margin: 50px auto;
}

h1 {
  background-color: #f44336;
  color: white;
  padding: 15px;
  margin: 0;
  text-align: center;
}

input[type="text"] {
  width: 75%;
  padding: 10px;
  border: none;
  outline: none;
}

button {
  width: 25%;
  padding: 10px;
  background-color: #ccc;
  border: none;
  cursor: pointer;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 15px;
  background-color: #f9f9f9;
  border-bottom: 1px solid #ddd;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li.completed {
  background-color: #ddd;
  text-decoration: line-through;
}

li span {
  cursor: pointer;
  flex-grow: 1;
}

li button {
  background-color: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
}
</style>