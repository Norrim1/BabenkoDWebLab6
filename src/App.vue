<template>
  <div class="container">
    <header class="app-header">
      <h1>Мои задачи</h1>
      <ThemeToggle @update-theme="setTheme" />
    </header>

    <SearchBar v-model:query="searchQuery" />

    <button @click="isModalOpen = true" class="btn">
      Добавить задачу
    </button>

    <TodoList
      :tasks="filteredAndSortedTasks"
      @toggle="toggleTask"
      @edit="openEditModal"
      @delete="taskIdToDelete = $event"
    />

    <AddTodoModal
      v-if="isModalOpen"
      @add="addTask"
      @close="isModalOpen = false"
    />

    <EditTodoModal
      v-if="isEditModalOpen"
      :task-id="editingTaskId"
      :current-title="editingTaskTitle"
      @update="updateTask"
      @close="isEditModalOpen = false"
    />

    <div v-if="taskIdToDelete" class="modal modal--active">
      <div class="modal__content">
        <span class="modal__close" @click="taskIdToDelete = null">&times;</span>
        <h2>Удалить задачу?</h2>
        <p>Это действие нельзя отменить.</p>
        <button class="btn" @click="deleteTask(taskIdToDelete)">
          Удалить
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import ThemeToggle from './components/ThemeToggle.vue'
import SearchBar from './components/SearchBar.vue'
import TodoList from './components/TodoList.vue'
import AddTodoModal from './components/AddTodoModal.vue'
import EditTodoModal from './components/EditTodoModal.vue'

const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || [])
const searchQuery = ref('')
const isModalOpen = ref(false)
const isEditModalOpen = ref(false)
const taskIdToDelete = ref(null)
const editingTaskId = ref(null) 
const editingTaskTitle = ref('')
const theme = ref(localStorage.getItem('theme') || 'light')

document.documentElement.setAttribute('data-theme', theme.value)

const filteredAndSortedTasks = computed(() => {
  const filtered = tasks.value.filter(task =>
    task.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
  return [...filtered].sort((a, b) => {
    if (a.completed && !b.completed) return 1
    if (!a.completed && b.completed) return -1
    return 0
  })
})

const setTheme = (newTheme) => {
  theme.value = newTheme
  document.documentElement.setAttribute('data-theme', newTheme)
  localStorage.setItem('theme', newTheme)
}

const addTask = (title) => {
  tasks.value.push({
    id: Date.now(),
    title,
    completed: false
  })
  isModalOpen.value = false
}

const toggleTask = (id) => {
  const task = tasks.value.find(t => t.id === id)
  if (task) task.completed = !task.completed
}

const openEditModal = (id) => {
  const task = tasks.value.find(t => t.id === id)
  if (task) {
    editingTaskId.value = id
    editingTaskTitle.value = task.title
    isEditModalOpen.value = true
  }
}

const updateTask = ({ id, title }) => {
  const task = tasks.value.find(t => t.id === id)
  if (task) {
    task.title = title
  }
  isEditModalOpen.value = false
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(t => t.id !== id)
  taskIdToDelete.value = null
}

watch(tasks, (newTasks) => {
  localStorage.setItem('tasks', JSON.stringify(newTasks))
}, { deep: true })
</script>