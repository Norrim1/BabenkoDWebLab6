<template>
    <div class="modal modal--active">
      <div class="modal__content">
        <span class="modal__close" @click="$emit('close')">&times;</span>
        <h2>Добавить задачу</h2>
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <input
              v-model="title"
              type="text"
              placeholder="Введите название..."
              required
              minlength="1"
              maxlength="100"
            />
          </div>
          <button type="submit" class="btn">Создать задачу</button>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const title = ref('')
  const emit = defineEmits(['add', 'close'])
  
  const handleSubmit = () => {
    if (title.value.trim()) {
      emit('add', title.value.trim())
      title.value = ''
    }
  }
  </script>