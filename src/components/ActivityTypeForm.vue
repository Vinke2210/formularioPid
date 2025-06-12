<script setup>
import { ref, onMounted } from 'vue'
import RichTextEditor from './RichTextEditor.vue'

const denominacion = ref('')
const fechaInicio = ref('01/10/2024')
const fechaFin = ref('')
const descripcion = ref('')

const denominacionError = ref('')
const fechaInicioError = ref('')

const validateForm = () => {
  denominacionError.value = ''
  fechaInicioError.value = ''
  
  let isValid = true
  
  if (!denominacion.value.trim()) {
    denominacionError.value = 'La denominación es obligatoria'
    isValid = false
  }
  
  if (!fechaInicio.value.trim()) {
    fechaInicioError.value = 'La fecha de inicio es obligatoria'
    isValid = false
  }
  
  return isValid
}

const handleCancelar = () => {
  // Limpiar formulario
  denominacion.value = ''
  fechaInicio.value = ''
  fechaFin.value = ''
  descripcion.value = ''
  denominacionError.value = ''
  fechaInicioError.value = ''
  console.log('Formulario cancelado')
}

const handleAplicar = () => {
  if (validateForm()) {
    const data = {
      denominacion: denominacion.value,
      fechaInicio: fechaInicio.value,
      fechaFin: fechaFin.value,
      descripcion: descripcion.value
    }
    console.log('Datos aplicados:', data)
    alert('Cambios aplicados correctamente')
  }
}

const handleAceptar = () => {
  if (validateForm()) {
    const data = {
      denominacion: denominacion.value,
      fechaInicio: fechaInicio.value,
      fechaFin: fechaFin.value,
      descripcion: descripcion.value
    }
    console.log('Datos aceptados:', data)
    alert('Tipo de actividad guardado correctamente')
    handleCancelar() // Limpiar después de guardar
  }
}

const updateDescripcion = (content) => {
  descripcion.value = content
}
</script>

<template>
  <div class="form-container">
    <div class="form-header">
      <h2>Adicionar tipo de actividad</h2>
      <div class="header-buttons">
        <button class="btn-icon" @click="handleCancelar" title="Cancelar">
          <span>×</span>
        </button>
      </div>
    </div>
    
    <div class="form-content">
      <div class="form-group">
        <label for="denominacion">Denominación *</label>
        <input 
          id="denominacion"
          v-model="denominacion"
          type="text" 
          class="form-input"
          :class="{ 'error': denominacionError }"
          placeholder="Ingrese la denominación"
        />
        <span v-if="denominacionError" class="error-message">{{ denominacionError }}</span>
      </div>
      
      <div class="form-row">
        <div class="form-group">
          <label for="fechaInicio">Fecha inicio *</label>
          <div class="date-input-container">
            <input 
              id="fechaInicio"
              v-model="fechaInicio"
              type="date" 
              class="form-input date-input"
              :class="{ 'error': fechaInicioError }"
            />
            <span class="calendar-icon">📅</span>
          </div>
          <span v-if="fechaInicioError" class="error-message">{{ fechaInicioError }}</span>
        </div>
        
        <div class="form-group">
          <label for="fechaFin">Fecha fin:</label>
          <div class="date-input-container">
            <input 
              id="fechaFin"
              v-model="fechaFin"
              type="date" 
              class="form-input date-input"
            />
            <span class="calendar-icon">📅</span>
          </div>
        </div>
      </div>
      
      <div class="form-group">
        <label>Descripción:</label>
        <RichTextEditor @update:content="updateDescripcion" />
      </div>
    </div>
    
    <div class="form-footer">
      <button class="btn btn-secondary" @click="handleCancelar">
        Cancelar
      </button>
      <button class="btn btn-primary" @click="handleAplicar">
        Aplicar
      </button>
      <div class="btn-group">
        <button class="btn btn-success" @click="handleAceptar">
          Aceptar
        </button>
        <button class="btn btn-success btn-dropdown">
          ▼
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.form-container {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 100%;
  max-width: 700px;
  overflow: hidden;
}

.form-header {
  background: linear-gradient(135deg, #8B5A96, #A569BD);
  color: white;
  padding: 12px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.form-header h2 {
  margin: 0;
  font-size: 16px;
  font-weight: 500;
}

.header-buttons {
  display: flex;
  gap: 8px;
}

.btn-icon {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: white;
  width: 24px;
  height: 24px;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  transition: background-color 0.2s;
}

.btn-icon:hover {
  background: rgba(255, 255, 255, 0.3);
}

.form-content {
  padding: 24px;
}

.form-group {
  margin-bottom: 20px;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 6px;
  font-weight: 500;
  color: #333;
  font-size: 14px;
}

.form-input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.2s, box-shadow 0.2s;
  box-sizing: border-box;
}

.form-input:focus {
  outline: none;
  border-color: #8B5A96;
  box-shadow: 0 0 0 2px rgba(139, 90, 150, 0.2);
}

.form-input.error {
  border-color: #dc3545;
}

.date-input-container {
  position: relative;
}

.date-input {
  padding-right: 40px;
}

.calendar-icon {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  color: #666;
  font-size: 16px;
}

.error-message {
  color: #dc3545;
  font-size: 12px;
  margin-top: 4px;
  display: block;
}

.form-footer {
  background: #f8f9fa;
  padding: 16px 24px;
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  border-top: 1px solid #e9ecef;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
}

.btn:hover {
  transform: translateY(-1px);
}

.btn:active {
  transform: translateY(0);
}

.btn-secondary {
  background: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background: #5a6268;
}

.btn-primary {
  background: #8B5A96;
  color: white;
}

.btn-primary:hover {
  background: #7a4e85;
}

.btn-success {
  background: #28a745;
  color: white;
}

.btn-success:hover {
  background: #218838;
}

.btn-group {
  display: flex;
}

.btn-group .btn {
  border-radius: 0;
}

.btn-group .btn:first-child {
  border-top-left-radius: 4px;
  border-bottom-left-radius: 4px;
}

.btn-group .btn:last-child {
  border-top-right-radius: 4px;
  border-bottom-right-radius: 4px;
}

.btn-dropdown {
  padding: 8px 10px;
  border-left: 1px solid rgba(255, 255, 255, 0.3);
}

@media (max-width: 768px) {
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .form-footer {
    flex-direction: column;
    gap: 8px;
  }
  
  .btn {
    width: 100%;
  }
  
  .btn-group {
    width: 100%;
  }
}
</style>