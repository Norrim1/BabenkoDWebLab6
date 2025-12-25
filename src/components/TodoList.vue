<template>
    <ul class="tasks-list">
      <template v-if="tasks.length">
        <TodoItem
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @toggle="$emit('toggle', $event)"
          @edit="$emit('edit', $event)"
          @delete="$emit('delete', $event)"
        />
      </template>
      <p v-else class="empty-message">Нет задач. Добавьте первую!</p>
    </ul>
  </template>
  
  <script setup>
  import TodoItem from './TodoItem.vue'
  
  defineProps({
    tasks: Array
  })
  defineEmits(['toggle', 'edit', 'delete'])
  </script>