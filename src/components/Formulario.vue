
<script setup>
import Alerta from './Alerta.vue'
import { ref, reactive, computed } from 'vue';


const alerta = reactive({
    tipo: '',
    mensaje: ''
})


const emit = defineEmits([
    'update:nombre',
    'update:propietario', 
    'update:email', 
    'update:alta', 
    'update:sintomas',
    'guardar-paciente'
])

const props = defineProps({
    nombre: {
        type: String,
        required: true
    },
    propietario: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    alta: {
        type: String,
        required: true
    },
    sintomas: {
        type: String,
        required: true
    },
    id: {
        type: [String, null],
        required: true
    }
})


const validar = () =>{
    console.log('validando..')

    // ASI EVITO VALIDAR UNA POR UNA LAS PROPIEDADES DE PACIENTE 
    if(Object.values(props).includes('')){
        alerta.mensaje = 'Todos los campos son obligarios'
        alerta.tipo= 'error'
        return
    }

    emit('guardar-paciente')
    alerta.mensaje = 'Paciente guardado correctamente'
    alerta.tipo = 'exito'

    setTimeout(() => {
        alerta.mensaje = ''
        alerta.tipo = ''
    }, 2000);
}


const editando = computed(()=>{

    return props.id
})



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
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                    />
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
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
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
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                    
                    />
                    
                </div>

            <div class="mb-5">
                <label for="date" class="block text-gray-700 uppercase font-bold">

                    Alta
                </label>

                <input 
                    id="date" 
                    type="date" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
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
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                    />
                </div>


            <input 
                type="submit" 
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover: bg-indigo 
                cursor:pointer transition-color" 
                :value="editando ? 'Guardar Cambios' : 'Registrar Paciente'">

        </form>
    </div>
</template>

<style></style>

<!-- FLUJO DE ALERTA

1. CREO EL COMPONENTE ALERTA QUE RECIBIRA UN PROP DE ALERTA 
2. CREO EL OBJETO ALERTA ACA EN EL FOMRULARIO 
3. TRAIGO EL COMPONENTE ALERTA
4. SI EL FORMULARIO TIENE ALGUN VACIO DISPARA ALERTA 
5. PASO LA ALERTA AL COMPONENTE ALERTA Y ADEMAS LE DOY UN V-IF AL COMPONENTE
SI HAY ALERTA MUESTRATE SI NO NO 
6. EL COMPONENTE ALERTA RECIBO LA PROPS  Y CREO UNA PROPIEDAD COMPUTADA 
LLAMADA ISERROR DONDE RETORNO LA PROPS DE ALERTA QUE ES IGUAL A ERROR 
7. SI LA PROIPIEDAD COMPUTADA ES TRUE, LE DOY LA CLASE ROJA SI NO LA VERDE -->


FLUJO DE LOS V.MODEL 

<!-- 1. SE TRASLADO EL OBJETO PACIENTE AL COMPONENTE PADRE APP.VUE 
2. SE PASARON POR EMITS LOS V-MODEL Y SE DEFINIERON COMO EMITS ACA ARRIBA COMO UPDATE:NOMBRE.. UPDATE..ETC 
3. SE DEFINE EN CADA INPUT EL @INPUT CON EL EMIT $emit('update:nombre', $event.target.value) 
4. SE DEFINEN LOS PROPS QUE SON LOS MISMOS DEL UPDATE EN ESTE CASO nombre Y CORRESPONDE A CADA EMIT -->