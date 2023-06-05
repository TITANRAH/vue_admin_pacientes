
<script setup>
import Alerta from './Alerta.vue'
import { ref, reactive } from 'vue';


const alerta = reactive({
    tipo: '',
    mensaje: ''
})



// si tuvera 5 datos diferentes convendria usar reactive para qye sea un objeto
// reactive siempre es un objeto
const paciente = reactive({

   nombre: '',
   propietario: '',
   email: '',
   alta: '',
   sintomas:''

})

const validar = () =>{
    console.log('validando..')

    // ASI EVITO VALIDAR UNA POR UNA LAS PROPIEDADES DE PACIENTE 
    if(Object.values(paciente).includes('')){
        alerta.mensaje = 'Todos los campos son obligarios'
        alerta.tipo= 'error'
        return
    }

    console.log('agregando')
}

</script>
<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y
            <span class="text-indigo-600 font-bold">
                Administralos
            </span>
        </p>

        <Alerta
            v-if="alerta.mensaje"
            :alerta="alerta"
        />
        <form @submit.prevent="validar" class="bg-white shadow-md rounded-lg py-10 px-5 mb-10">
            <div class="mb-5">
               
                <label for="mascota" class="block text-gray-700 uppercase font-bold">

                    Nombre Mascota
                </label>

                <input 
                    id="mascota" 
                    type="text" 
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    v-model="paciente.nombre"/>
            </div>

            <div class="mb-5">

                <label for="propietario" class="block text-gray-700 uppercase font-bold">

                    Nombre Propietario
                </label>

                <input 
                    id="propietario" 
                    type="text" 
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    v-model="paciente.propietario"
                    />
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">

                    Email
                </label>

                <input 
                    id="email" 
                    type="text" 
                    placeholder="Email del propietario"
                    v-model="paciente.email"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" />
            </div>

            <div class="mb-5">
                <label for="date" class="block text-gray-700 uppercase font-bold">

                    Alta
                </label>

                <input 
                    id="date" 
                    type="date" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    v-model="paciente.alta"
                    />
                    
                </div>
            <div class="mb-5">
                <label for="date" class="block text-gray-700 uppercase font-bold">

                    Síntomas
                </label>

                <textarea 
                    id="date" 
                    placeholder="Describe los síntomas" 
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" 
                    v-model="paciente.sintomas"
                    />
                </div>


            <input 
                type="submit" 
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover: bg-indigo 
                cursor:pointer transition-color" 
                value="Registrar paciente">

        </form>
    </div>
</template>

<style></style>