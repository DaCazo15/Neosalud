<script setup>
    import { reactive } from 'vue';
    import Alerta from './Alerta.vue'; 

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emits =defineEmits([
        'update:mascota',
        'update:propietario',
        'update:email',
        'update:alta',
        'update:sintomas',
        'guardar-paciente'
    ])

    const props = defineProps({
        mascota:{ type: String, required: true },
        propietario:{ type: String, required: true },
        email:{ type: String, required: true },
        alta:{ type: String, required: true },
        sintomas:{ type: String, required: true }
    })

    const agregarPaciente = e =>{
        if (Object.values(props).includes('')){
            alerta.mensaje = 'Todos los campos son obligatorios';
            alerta.tipo = 'error';
            return
        }else{
            alerta.mensaje = 'Paciente agregado correctamente';
            alerta.tipo = 'success';
            emits('guardar-paciente');
        }
    }

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="text-3xl text-center font-black">Seguimiento Pacientes</h2>

        <p class="text-center text-lg mb-10">
            Añadir pacientes
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <Alerta 
            v-if="alerta.mensaje"
            :alerta="alerta"
        />

        <form 
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            v-on:submit.prevent="agregarPaciente"
        >
            <div class="mb-5">
                <label 
                    for="mascota"
                    class="block text-gray-900 font-bold mb-2 ml-2 uppercase"
                >
                    NOMBRE MASCOTA
                </label>
                <input 
                    type="text"
                    id="mascota"
                    placeholder="Nombre de la mascota"
                    class="placeholder-gray-400 rounded-md border-2 w-full p-3"
                    :value="mascota"
                    @input="$emit('update:mascota', $event.target.value)"
                    >
                </div>
                
            <div class="mb-5">
                <label 
                    for="propietario"
                    class="block text-gray-900 font-bold mb-2 ml-2 uppercase"
                    >
                    Nombre Propietario
                </label>
                <input 
                    type="text"
                    id="propietario"
                    placeholder="Nombre del propietario"
                    class="placeholder-gray-400 rounded-md border-2 w-full p-3"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                >       
            </div>

            <div class="mb-5">
                <label 
                    for="email"
                    class="block text-gray-900 font-bold mb-2 ml-2 uppercase"
                >
                    Email
                </label>
                <input 
                    type="email"
                    id="email"
                    placeholder="Email del propietario"
                    class="placeholder-gray-400 rounded-md border-2 w-full p-3"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                    >
                </div>
                
                <div class="mb-5">
                    <label 
                    for="alta"
                    class="block text-gray-900 font-bold mb-2 ml-2 uppercase"
                >
                    Fecha de Alta
                </label>
                <input 
                    type="date"
                    id="alta"
                    class="placeholder-gray-400 rounded-md border-2 w-full p-3"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                    for="sintomas"
                    class="block text-gray-900 font-bold mb-2 ml-2 uppercase"
                >
                    Síntomas
                </label>
                <textarea
                    id="sintomas"
                    placeholder="Describe los sintomas de la mascota..."
                    class="placeholder-gray-400 rounded-md border-2 w-full p-3 h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                ></textarea>

                <input 
                    type="submit" 
                    value="Agregar Paciente" 
                    class="bg-indigo-600 hover:bg-indigo-700 text-2xl text-white mt-4 p-3 rounded-md w-full uppercase font-bold cursor-pointer transition-colors"
                    >
            </div>
        </form>
    </div>
</template>


