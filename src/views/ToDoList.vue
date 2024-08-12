<template>
  <div>
    <h1>To Do List</h1>
    <input v-model="newTodo" placeholder="Add a new to-do"/>
    <button @click="addTodo">Add</button>
    <div>
      <input type="checkbox" v-model="checkAll" @change="toggleAll"/> Check All
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <input type="checkbox" v-model="todo.completed" @change="toggleTodo(index)"/>
        <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">{{ todo.text }}</span>
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import {ref, watch} from 'vue'

// State
const newTodo = ref('')
const todos = ref<{ text: string, completed: boolean }[]>([])
const checkAll = ref(false)

// Methods
const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({text: newTodo.value.trim(), completed: false})
    newTodo.value = ''
  }
}

const removeTodo = (index: number) => {
  todos.value.splice(index, 1)
}

const toggleTodo = (index: number) => {
  todos.value[index].completed = !todos.value[index].completed
}

const toggleAll = () => {
  const newStatus = !checkAll.value
  todos.value.forEach(todo => {
    todo.completed = newStatus
  })
  checkAll.value = newStatus
}

// Watchers
watch(todos, (newTodos) => {
  checkAll.value = newTodos.length > 0 && newTodos.every(todo => todo.completed)
}, {deep: true})
</script>

<style scoped>
/* Add your styles here */
</style>