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
      <li v-if="filteredTodos.length === 0" class="empty">Belum ada tugas </li>
    </ul>

    <footer class="footer">Made by syntia adhisti </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const filter = ref('all')

const todos = ref([
  // Kosongkan kalau ingin test kosong: []
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
  background: linear-gradient(145deg, #f8e1f4, #e1d5f8);
  font-family: 'Segoe UI', sans-serif;
}

.container {
  max-width: 400px;
  margin: 50px auto;
  background: #f5f1fa;
  border-radius: 20px;
  padding: 24px;
  box-shadow: 8px 8px 20px #d4cce0, -8px -8px 20px #ffffff;
  text-align: center;
}

h1 {
  color: #5e4b8b;
  margin-bottom: 20px;
}

.form {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 12px;
  border: none;
  border-radius: 12px;
  background: #f0eafc;
  box-shadow: inset 4px 4px 8px #d6cfe0, inset -4px -4px 8px #ffffff;
}

button {
  padding: 12px 20px;
  background: linear-gradient(to right, #d291bc, #c48ade);
  border: none;
  border-radius: 12px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: linear-gradient(to right, #c48ade, #d291bc);
}

.dropdown-filter {
  margin-bottom: 20px;
  text-align: left;
}

select {
  padding: 10px;
  border-radius: 12px;
  border: none;
  background: #f0eafc;
  box-shadow: inset 3px 3px 6px #d6cfe0, inset -3px -3px 6px #ffffff;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  display: flex;
  align-items: center;
  background: #ffffff;
  border-radius: 12px;
  margin-bottom: 12px;
  padding: 12px;
  box-shadow: 4px 4px 10px #d4cce0, -4px -4px 10px #ffffff;
}

li.done span {
  text-decoration: line-through;
  color: #999;
}

li span {
  flex: 1;
  margin-left: 10px;
  font-size: 16px;
}

input[type="checkbox"] {
  transform: scale(1.2);
}

.delete {
  background: none;
  border: none;
  font-size: 20px;
  color: #d291bc;
  cursor: pointer;
  transition: transform 0.2s ease, color 0.3s ease;
}

.delete:hover {
  color: #b46cad;
  transform: scale(1.2);
}

li.empty {
  justify-content: center;
  font-style: italic;
  color: #bba9cc;
  padding: 20px;
}

.footer {
  margin-top: 30px;
  font-size: 12px;
  color: #aaa;
  animation: blink 2.5s infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
</style>
