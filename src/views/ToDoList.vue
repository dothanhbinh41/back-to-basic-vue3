<template>
  <div>
    <h1>Languages developer</h1>
    <div style="display: flex;">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Title...">
      <button @click="addTodo">Add</button>
    </div>
    <div v-if="todos.length > 0">
      <input type="checkbox" @change="toggleCheckAll" v-model="allSelected">
      <label for="check-all">Check All</label>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index" :class="{ completed: todo.completed }">
        <input type="checkbox" v-model="todo.selected">
        <span @click="toggleComplete(todo)">{{ todo.text }}</span>
        <button @click="removeTodo(index)">×</button>
      </li>
    </ul>
    <button v-if="hasSelectedTodos" @click="clearSelected"
            style="margin-top: 20px; padding: 10px; background-color: #f44336; color: white; border: none; cursor: pointer;">
      Clear
    </button>
  </div>
</template>

<script>
import {ref, watch, computed} from 'vue';

export default {
  setup() {
    const newTodo = ref('');
    const allSelected = ref(false);
    const todos = ref([
      {text: 'Laravel', completed: false, selected: false},
      {text: 'Vue', completed: false, selected: false},
      {text: 'React', completed: false, selected: false},
      {text: 'C#', completed: false, selected: false},
      {text: 'Python', completed: false, selected: false},
      {text: 'GoLang', completed: false, selected: false}
    ]);

    const clearTodos = () => {
      todos.value = []; // Xóa tất cả các mục trong danh sách
    };

    const clearSelected = () => {
      todos.value = todos.value.filter(todo => !todo.selected); // Lọc ra các mục không được chọn
    };

    const toggleCheckAll = () => {
      todos.value.forEach(todo => {
        todo.selected = allSelected.value;
      });
    };

    const hasSelectedTodos = computed(() => {
      return todos.value.some(todo => todo.selected);
    });


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

    watch(todos, () => {
      allSelected.value = todos.value.every(todo => todo.selected);
    }, {deep: true});

    return {
      newTodo,
      todos,
      clearTodos,
      clearSelected,
      addTodo,
      removeTodo,
      toggleComplete,
      allSelected,
      toggleCheckAll,
      hasSelectedTodos
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

input[type="checkbox"] {
  margin-right: 5px;
}
</style>