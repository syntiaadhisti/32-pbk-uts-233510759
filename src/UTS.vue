<template>
  <div class="container">
    <h1>📋 Todo List</h1>

    <form @submit.prevent="addTodo" class="form">
      <input v-model="newTodo" placeholder="Tambahkan tugas baru..." />
      <button type="submit">+ Tambah</button>
    </form>

    <div class="dropdown-filter">
      <select v-model="filter">
        <option value="all">Semua</option>
        <option value="pending">Belum Selesai</option>
        <option value="completed">Selesai</option>
      </select>
    </div>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="index"
        :class="{ done: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button class="delete" @click="removeTodo(index)">🗑️</button>
      </li>
      <li v-if="filteredTodos.length === 0" class="empty">Belum ada tugas</li>
    </ul>

    <footer class="footer">Made by syntia adhisti</footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  { text: 'makan', completed: false },
  { text: 'melukis', completed: false },
  { text: 'main game', completed: true }
])

const addTodo = () => {
  if (newTodo.value.trim() === '') return
  todos.value.push({ text: newTodo.value.trim(), completed: false })
  newTodo.value = ''
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'pending') return todos.value.filter(t => !t.completed)
  if (filter.value === 'completed') return todos.value.filter(t => t.completed)
  return todos.value
})
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
}

.container {
  max-width: 400px;
  margin: 50px auto;
  background: white;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 16px;
  background-color: #d745bd;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #d246bd;
}

.dropdown-filter {
  margin-bottom: 20px;
  text-align: left;
}

select {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

li.done span {
  text-decoration: line-through;
  color: #888;
}

li span {
  flex: 1;
  margin-left: 10px;
}

input[type="checkbox"] {
  transform: scale(1.2);
}

.delete {
  background: none;
  border: none;
  font-size: 16px;
  color: red;
  cursor: pointer;
}

li.empty {
  text-align: center;
  padding: 20px;
  font-style: italic;
  color: #888;
}

.footer {
  margin-top: 30px;
  font-size: 12px;
  color: #999;
}
</style>
