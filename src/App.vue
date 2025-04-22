<template>
  <main class="max-w-xl mx-auto mt-10 p-4">
    <TaskInput @add-task="agregarTarea" />

    <!-- Tareas pendientes -->
    <section class="mt-6">
      <h2 class="text-lg font-bold mb-2">Tareas pendientes</h2>
      <ul v-if="tareasPendientes.length">
        <li
          v-for="(tarea, index) in tareasPendientes"
          :key="index"
          class="flex justify-between items-center mb-2"
        >
          <span>{{ tarea.texto }}</span>
          <button
            @click="marcarComoCompletada(index)"
            class="text-green-600 hover:text-green-800"
            title="Marcar como completada"
          >
            <font-awesome-icon icon="check" />
          </button>
        </li>
      </ul>
      <p v-else class="text-gray-500">No hay tareas pendientes.</p>
    </section>

    <!-- Tareas completadas -->
    <section class="mt-6">
      <h2 class="text-lg font-bold mb-2">Tareas completadas</h2>
      <ul v-if="tareasCompletadas.length">
        <li
          v-for="(tarea, index) in tareasCompletadas"
          :key="'completa-' + index"
          class="flex justify-between items-center mb-2"
        >
          <span class="text-gray-500 line-through">{{ tarea.texto }}</span>
          <button
            @click="eliminarCompletada(index)"
            class="text-red-600 hover:text-red-800"
            title="Eliminar tarea"
          >
            <font-awesome-icon icon="trash" />
          </button>
        </li>
      </ul>
      <p v-else class="text-gray-500">Todavía no completaste ninguna tarea.</p>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue'
import TaskInput from './components/TaskInput.vue'

interface Tarea {
  texto: string
  completada: boolean
}

// Cargar tareas desde localStorage si existen
const tareas = ref<Tarea[]>(JSON.parse(localStorage.getItem('tareas') || '[]'))

// Guardar tareas en localStorage cada vez que cambian
watchEffect(() => {
  localStorage.setItem('tareas', JSON.stringify(tareas.value))
})

function agregarTarea(nuevaTarea: string) {
  tareas.value.push({ texto: nuevaTarea, completada: false })
}

function marcarComoCompletada(index: number) {
  const tarea = tareasPendientes.value[index]
  const i = tareas.value.findIndex((t) => t === tarea)
  if (i !== -1) tareas.value[i].completada = true
}

function eliminarCompletada(index: number) {
  const tarea = tareasCompletadas.value[index]
  tareas.value = tareas.value.filter((t) => t !== tarea)
}

const tareasPendientes = computed(() =>
  tareas.value.filter((t) => !t.completada)
)

const tareasCompletadas = computed(() =>
  tareas.value.filter((t) => t.completada)
)
</script>

<style scoped>
</style>