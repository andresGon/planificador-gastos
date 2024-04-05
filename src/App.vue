<script setup>
import { ref, reactive } from 'vue'
import Presupuesto from './components/Presupuesto.vue';
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg'


const modal = reactive({
  mostrar:false,
  animar:false
})

const presupuesto =ref(0)
const disponible = ref(0)

const definirPresupuesto = (cantidad) => {
  presupuesto.value=cantidad
  console.log(' definiendo presupuesto');
}
</script>

<template>
  <div class="container">
    <h1>Planificador de gastos</h1>
    <Presupuesto
      v-if="presupuesto === 0"
      @definir-presupuesto="definirPresupuesto"
    />
    <ControlPresupuesto
      v-else
      :presupuesto="presupuesto"
      :disponible="disponible"
    />

    <div class="crear-gasto">
      <img :src="iconoNuevoGasto" alt="">
    </div>
  </div>
 
</template>

<style scoped>
.container{
  text-align: center;
  width: 100%;
}
.crear-gasto{
  width: 24px;
}
</style>
