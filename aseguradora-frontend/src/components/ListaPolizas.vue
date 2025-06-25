<template>
  <div class="w-full max-w-4xl mx-auto mt-10 p-4">
    <div class="bg-white shadow-md rounded-lg overflow-hidden">
      <h2 class="text-xl font-bold text-gray-800 p-4 border-b">📋 Pólizas Existentes</h2>
      <div v-if="mensaje" class="p-4 text-center text-gray-600">{{ mensaje }}</div>
      <table v-else-if="polizas.length" class="min-w-full leading-normal">
        <thead>
          <tr>
            <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
              Número Póliza
            </th>
            <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
              Titular
            </th>
            <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
              Tipo
            </th>
            <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
              Monto
            </th>
            <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
              Acciones
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="poliza in polizas" :key="poliza._id">
            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">{{ poliza.numeroPoliza }}</td>
            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">{{ poliza.titular }}</td>
            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">{{ poliza.tipoSeguro }}</td>
            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">${{ poliza.monto.toLocaleString() }}</td>
            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">

                <button @click="$emit('editar-poliza', poliza)" class="text-blue-600 hover:text-blue-900">Editar</button>
            </td>
          </tr>
        </tbody>
      </table>
      <div v-else class="p-4 text-center text-gray-500">No hay pólizas para mostrar.</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

defineEmits(['editar-poliza'])

const polizas = ref([])
const mensaje = ref('Cargando pólizas...')

const obtenerPolizas = async () => {
  try {
    const res = await axios.get('http://localhost:4000/api/polizas')
    polizas.value = res.data
    mensaje.value = ''
  } catch (error) {
    console.error(error)
    mensaje.value = 'Error al cargar las pólizas.'
  }
}

onMounted(() => {
  obtenerPolizas()
})
</script>