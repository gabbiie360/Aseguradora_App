<template>
  <div class="bg-white p-8 rounded-xl shadow-lg w-full max-w-md mx-auto">
    <!-- Cambiado: El título ahora es dinámico -->
    <h2 class="text-2xl font-bold text-center text-gray-800 mb-6">{{ modoEdicion ? '✍️ Editar Póliza' : '📄 Crear nueva póliza' }}</h2>
    <form @submit.prevent="enviarPoliza">
        <div class="mb-4">
          <label for="numeroPoliza" class="block text-gray-700 text-sm font-bold mb-2">Número de póliza</label>
          <input id="numeroPoliza" v-model="poliza.numeroPoliza" required class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" />
        </div>
        <div class="mb-4">
          <label for="tipoSeguro" class="block text-gray-700 text-sm font-bold mb-2">Tipo de seguro</label>
          <select id="tipoSeguro" v-model="poliza.tipoSeguro" required class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500">
            <option disabled value="">Seleccione un tipo</option>
            <option>Auto</option>
            <option>Vida</option>
            <option>Hogar</option>
            <option>Salud</option>
          </select>
        </div>
        <div class="mb-4">
          <label for="titular" class="block text-gray-700 text-sm font-bold mb-2">Titular</label>
          <input id="titular" v-model="poliza.titular" required class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" />
        </div>
        <div class="mb-6">
          <label for="monto" class="block text-gray-700 text-sm font-bold mb-2">Monto</label>
          <input id="monto" v-model.number="poliza.monto" type="number" required class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500" />
        </div>
      <button type="submit" :class="[modoEdicion ? 'bg-blue-500 hover:bg-blue-700' : 'bg-green-500 hover:bg-green-700']" class="w-full text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline transition-colors duration-300">
        {{ modoEdicion ? '💾 Actualizar' : '💾 Guardar' }}
      </button>
      <button v-if="modoEdicion" @click="cancelarEdicion" type="button" class="mt-2 w-full bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline transition-colors duration-300">
        Cancelar
      </button>
    </form>
    <p v-if="mensaje" class="mt-4 text-center font-semibold text-gray-700">{{ mensaje }}</p>
  </div>
</template>

<script setup>
import { ref, watch, computed } from 'vue'
import axios from 'axios'

const props = defineProps({
  polizaAEditar: {
    type: Object,
    default: null
  }
})
const emit = defineEmits(['poliza-guardada', 'edicion-cancelada'])

const poliza = ref({
  numeroPoliza: '',
  tipoSeguro: '',
  titular: '',
  monto: null
})
const mensaje = ref('')

const modoEdicion = computed(() => !!props.polizaAEditar)


watch(() => props.polizaAEditar, (nuevaPoliza) => {
  if (nuevaPoliza) {
    poliza.value = { ...nuevaPoliza }
  } else {
    // Resetear el formulario si no hay póliza para editar
    poliza.value = { numeroPoliza: '', tipoSeguro: '', titular: '', monto: null }
  }
})

const enviarPoliza = async () => {
  mensaje.value = 'Guardando...'
  try {
    let res;
    if (modoEdicion.value) {
      // MODO EDICIÓN: Usamos PUT
      res = await axios.put(`http://localhost:4000/api/polizas/${props.polizaAEditar._id}`, poliza.value)
    } else {
      // MODO CREACIÓN: Usamos POST
      res = await axios.post('http://localhost:4000/api/polizas', poliza.value)
    }
    
    mensaje.value = res.data.msg || '¡Operación exitosa!'
    emit('poliza-guardada') 
    
  } catch (error) {
    mensaje.value = error.response?.data?.msg || 'Error en la operación'
  } finally {
    setTimeout(() => mensaje.value = '', 3000)
  }
}

const cancelarEdicion = () => {
  emit('edicion-cancelada');
}
</script>