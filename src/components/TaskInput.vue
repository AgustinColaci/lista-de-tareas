<template>
    <form @submit.prevent="handleSubmit" class="space-y-2">
      <h2 class="text-xl font-semibold">Listado de tareas</h2>
      <label for="task" class="block text-sm font-medium">Ingresar tarea:</label>
      <input
        v-model="text"
        id="task"
        placeholder="Ej. Limpiar la casa..."
        class="border px-3 py-1 rounded w-full"
      />
      <button
        type="submit"
        class="bg-blue-600 text-white px-4 py-1 rounded hover:bg-blue-700"
      >
        Agregar
      </button>
    </form>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  const text = ref('')
  
  // Emitimos un evento para el componente padre
  const emit = defineEmits<{
    (e: 'add-task', value: string): void
  }>()
  
  function handleSubmit() {
    if (text.value.trim()) {
      emit('add-task', text.value.trim())
      text.value = '' // Limpiar input
    }
  }
  </script>
  
  <style scoped>

  </style>