<script setup>
  import { ref, reactive } from 'vue';
  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Pacientes from './components/Pacientes.vue';

  const pacientes = ref([])
  

  const state = reactive({ 
    mascota: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
  })

  const guardarPaciente = () => {
    pacientes.value.push({state})
  }

</script>

<template>
  <div class=" container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario 
        v-model:nombre="state.mascota"
        v-model:propietario="state.propietario"
        v-model:email="state.email"
        v-model:alta="state.alta"
        v-model:sintomas="state.sintomas"
        @guardar-paciente="guardarPaciente"
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
            v-for="paciente in pacientes"
            :paciente="paciente"
          />
        </div>
        
      </div>
    </div>
  </div>
</template>

