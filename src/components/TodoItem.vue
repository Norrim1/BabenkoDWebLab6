<template>
    <li class="task-item" :class="{ completed: task.completed }" :data-id="task.id">
      <input
        type="checkbox"
        class="task-checkbox"
        :checked="task.completed"
        @change="$emit('toggle', task.id)"
      />
      <span class="task-title">{{ task.title }}</span>
      <div class="task-actions">
        <button class="task-btn edit" @click="$emit('edit', task.id)">Редактировать</button>
        <button class="task-btn delete" @click="$emit('delete', task.id)">Удалить</button>
      </div>
    </li>
  </template>
  
  <script setup>
  defineProps({
    task: Object
  })
  defineEmits(['toggle', 'edit', 'delete'])
  </script>