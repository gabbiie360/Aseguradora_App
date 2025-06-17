<template>
  <div class="main">
    <div class="card">
      <h2>📄 Crear nueva póliza</h2>
      <form @submit.prevent="enviarPoliza">
        <label>Número de póliza</label>
        <input v-model="poliza.numeroPoliza" required />

        <label>Tipo de seguro</label>
        <select v-model="poliza.tipoSeguro" required>
        <option disabled value="">Seleccione un tipo</option>
        <option>Auto</option>
        <option>Vida</option>
        <option>Hogar</option>
        <option>Salud</option>
        </select>


        <label>Titular</label>
        <input v-model="poliza.titular" required />

        <label>Monto</label>
        <input v-model.number="poliza.monto" type="number" required />

        <button type="submit">💾 Guardar</button>
      </form>
      <p v-if="mensaje" class="mensaje">{{ mensaje }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const poliza = ref({
  numeroPoliza: '',
  tipoSeguro: '',
  titular: '',
  monto: null
})

const mensaje = ref('')

const enviarPoliza = async () => {
  try {
    const res = await axios.post('http://localhost:4000/api/polizas', poliza.value)
    mensaje.value = res.data.msg
    poliza.value = { numeroPoliza: '', tipoSeguro: '', titular: '', monto: null }
  } catch (error) {
    mensaje.value = error.response?.data?.msg || 'Error al guardar póliza'
  }
}
</script>

<style scoped>
.main {
  min-height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #74ebd5, #acb6e5);
  font-family: 'Segoe UI', sans-serif;
  padding: 1rem;
  overflow-x: hidden; /* <-- Esto elimina el scroll horizontal */
}



.card {
  background: white;
  padding: 2rem;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  width: 100%;
  max-width: 480px;
}

.card h2 {
  margin-bottom: 1rem;
  font-size: 1.5rem;
  color: #2c3e50;
  text-align: center;
}

form {
  display: flex;
  flex-direction: column;
}

label {
  font-size: 0.95rem;
  margin-top: 1rem;
  color: #34495e;
}

input {
  padding: 0.6rem;
  margin-top: 0.3rem;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
  background: #f7f7f7;
  font-size: 1rem;
}

input:focus {
  border-color: #2980b9;
}

button {
  margin-top: 1.8rem;
  background-color: #27ae60;
  border: none;
  padding: 0.8rem;
  border-radius: 8px;
  color: white;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background-color: #2ecc71;
}

.mensaje {
  margin-top: 1.5rem;
  text-align: center;
  font-weight: bold;
  color: #2c3e50;
}
</style>
