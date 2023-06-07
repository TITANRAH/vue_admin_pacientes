<script setup>
import Header from './components/Headers.vue'
import Formulario from './components/Formulario.vue'
import Paciente from './components/Paciente.vue'
import { ref, reactive, watch, onMounted } from 'vue';
import {uid} from 'uid'
const pacientes = ref([])


watch(pacientes, ()=>{
    guardarLocalStorage()
},{
    deep: true
})

onMounted(() => {

 

    const pacientesStorage = localStorage.getItem('pacientes')
    if (pacientesStorage) {
        pacientes.value = JSON.parse(pacientesStorage)
    }
})

const guardarLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
}


// si tuvera 5 datos diferentes convendria usar reactive para qye sea un objeto
// reactive siempre es un objeto
const paciente = reactive({

  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: ''

})

const guardarPaciente = () => {

  // al guardar el id esta vacio si editamos no esta vacio
  // por lo tanto asi evitamos que al editar cree un registro nuevo
  if(paciente.id) {
    //encuentro el id por lo tanto al paciente 
    const { id } = paciente
    // digo que busque en los pacientes el ido de paciente state 
    const i = pacientes.value.findIndex((pacienteState)=>pacienteState.id === id)
    // una vez encuentro el paciente le digo que el paciente en la posicion en contrada 
    // es igual al paciente que esta siendo editado
    pacientes.value[i] = {...paciente}
  } else {
  // asi puedo ver el paciente y evitar que se reinicie a vacio que es lo que tambi9en necesito, con esto 
  // mantengo la copia del estado
  pacientes.value.push({
    ...paciente, id:uid()
  })
  }
  // paciente.nombre = ''
  // paciente.email = ''
  // paciente.propietario = ''
  // paciente.alta = ''
  // paciente.sintomas = ''

  // OTRA FORMA DE REINICIAR EL FORMULARIO YA SEA DESPUES DEL REGISTRO O DESPUES DE LA EDICION
  Object.assign(paciente, {
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
    id: null
  })
  
  }

  const actualizarPaciente = (id)=> {
    // al hacer el filtro y traer la posicion cero concvierto a pacienteEditar en un objeto
    const pacienteEditar = pacientes.value.filter( p=> p.id === id)[0]

    // con esto asigno este paciente encontrado al objeto paciente 
    Object.assign(paciente, pacienteEditar)

  }

  const eliminarPaciente = (id)=> {
   pacientes.value = pacientes.value.filter((p)=>p.id != id )
  }
</script>

<template>
  <div class="container mx-auto mt-20">

    <Header />

    <!-- el evento guardar-paciente  lo activo en el formulario pero vive aca en su padre -->
    <div class="mt-12 md:flex">
      <Formulario 
      v-model:alta="paciente.alta" 
      v-model:email="paciente.email" 
      v-model:propietario="paciente.propietario"
      v-model:nombre="paciente.nombre" 
      v-model:sintomas="paciente.sintomas" 
      @guardar-paciente="guardarPaciente" 
      :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center"> Información de </h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Pacientes
            <span class="text-indigo-600 font-bold">
              Administralos
            </span>
          </p>
          <Paciente 
          v-for="paciente in pacientes" 
          :paciente="paciente" 
          @actualizar-paciente="actualizarPaciente"
          @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="mt-10 text-2xl text-center">No hay pacientes </p>
      </div>
    </div>

  </div>
</template>
