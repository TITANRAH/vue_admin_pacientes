<script setup>
import Header from './components/Headers.vue'
import Formulario from './components/Formulario.vue'


</script>

<template>
  <div class="container mx-auto mt-20">

    <Header />

    <div class="mt-12 md:flex">
      <Formulario />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll"></div>
    </div>

  </div>
</template>
