<template>
    <div class="search-box">
      <input
        type="text"
        :value="query"
        @input="$emit('update:query', $event.target.value)"
        placeholder="Поиск задач по названию..."
      />
    </div>
  </template>
  
  <script setup>
  defineProps({
    query: String
  })
  defineEmits(['update:query'])
  </script>