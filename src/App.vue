<script setup>
  import { ref, reactive, onMounted, watch } from 'vue';
  import { uid } from 'uid';

  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Pacientes from './components/Pacientes.vue';

  const pacientes = ref([])

  watch(pacientes, () => { // Vigila cambios
      localStoragePacientes()
  }, { deep: true })

  const localStoragePacientes = () => { // Guardar en localStoragePacientes
      localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  onMounted (() => { // Cargar desde localStorage
    const pacientesStorage = localStorage.getItem('pacientes')
    if (pacientesStorage){
        pacientes.value = JSON.parse(pacientesStorage)
    }
  })
  
  const state = reactive({ 
    id: null,
    mascota: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
  })

  const limpiarFormulario = () => {
    Object.assign(state, {
      id: null,
      mascota: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: ''
    })
  }
  const guardarPaciente = () => {
    if (state.id) {
      const index = pacientes.value.findIndex(p => p.id === state.id)
      pacientes.value[index] = { ...state }
      limpiarFormulario()
      return
    }else{
      pacientes.value.push({ ...state, id: uid() })
      limpiarFormulario()
    }
  }
  
  const actualizarPaciente = (id) => {
    const paciente = pacientes.value.filter(p => p.id === id)[0]
    Object.assign(state, paciente)
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(p => p.id !== id)
  }

</script>

<template>
  <div class=" container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario 
        v-model:mascota="state.mascota"
        v-model:propietario="state.propietario"
        v-model:email="state.email"
        v-model:alta="state.alta"
        v-model:sintomas="state.sintomas"
        @guardar-paciente="guardarPaciente"
        :id = "state.id"
      />

      <div class="md:w-1/2 lg:w-3/5 md:h-screen overflow-y-scroll">
        <h3 class="text-3xl font-black text-center">Listado de Pacientes</h3>
        <div v-if="pacientes.length === 0" class="mt-5 text-center">
          <p class="text-xl text-gray-800 -mt-5">
            No hay pacientes <span class="font-bold text-indigo-600">Registrados</span>
          </p>
          <p class="text-gray-600 mt-12 text-4xl ">Comienza agregando pacientes</p>
        </div>
        <div v-else>
          <div class="mt-5 text-center">
            <p class="text-xl text-gray-800 -mt-5">
              Pacientes <span class="font-bold text-indigo-600">Registrados</span>
            </p>
          </div>
          <Pacientes
            v-for="(paciente, index) in pacientes"
            :key="index"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente(paciente.id)"
            @eliminar-paciente="eliminarPaciente(paciente.id)"
          />
        </div>
        
      </div>
    </div>
  </div>
</template>

