<template>
    <button class="btn" @click="toggleTheme">Переключить тему</button>
  </template>
  
  <script setup>
  const emit = defineEmits(['updateTheme'])
  
  const toggleTheme = () => {
    const current = document.documentElement.getAttribute('data-theme')
    const newTheme = current === 'dark' ? 'light' : 'dark'
    emit('updateTheme', newTheme)
  }
  </script>