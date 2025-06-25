<template>
  <main class="min-h-screen w-full bg-gradient-to-r from-teal-200 to-blue-300 font-sans p-4">
    <div class="container mx-auto space-y-8">
      <!-- Componente para crear/editar pólizas -->
      <CrearPoliza 
        :poliza-a-editar="polizaEnEdicion"
        @poliza-guardada="recargarLista"
        @edicion-cancelada="limpiarEdicion"
      />

      <ListaPolizas 
        :key="listaKey"
        @editar-poliza="prepararEdicion"
        @poliza-eliminada="recargarLista" 
      />
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import CrearPoliza from './components/CrearPoliza.vue'
import ListaPolizas from './components/ListaPolizas.vue'

const listaKey = ref(0)
const polizaEnEdicion = ref(null)

const prepararEdicion = (poliza) => {
  polizaEnEdicion.value = poliza
  window.scrollTo(0, 0)
}

const recargarLista = () => {
  limpiarEdicion()
  listaKey.value++
}

const limpiarEdicion = () => {
  polizaEnEdicion.value = null
}
</script>