<template>
    <div class="modal modal--active">
      <div class="modal__content">
        <span class="modal__close" @click="emit('close')">&times;</span>
        <h2>Редактировать задачу</h2>
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <input
              v-model="localTitle"
              type="text"
              placeholder="Новое название..."
              required
              minlength="1"
              maxlength="30"
              ref="inputRef"
            />
          </div>
          <button type="submit" class="btn">Сохранить</button>
        </form>
      </div>
    </div>
</template>
  
<script setup>
import { ref, watch } from 'vue'
  
  const props = defineProps({
    taskId: Number,
    currentTitle: String
  })
  
  const emit = defineEmits(['update', 'close'])

  const localTitle = ref(props.currentTitle)

  const inputRef = ref(null)
  watch(() => props.taskId, () => {
    localTitle.value = props.currentTitle
    nextTick(() => {
      inputRef.value?.focus()
    })
  })
  
  const handleSubmit = () => {
    if (localTitle.value.trim()) {
      emit('update', {
        id: props.taskId,
        title: localTitle.value.trim()
      })
    }
  }
  
  import { nextTick } from 'vue'
  </script>