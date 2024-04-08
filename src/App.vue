<script setup>
import { ref, reactive } from 'vue'
import Presupuesto from './components/Presupuesto.vue';
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg';
import Modal from './components/Modal.vue';


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

const mostrarModal = ()=>{
  modal.mostrar = true
  modal.animar = true
}

const ocultarModal = ()=>{
  modal.mostrar = false
  modal.animar = false
}


const gasto = reactive({
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now()
  })
  
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
      <img :src="iconoNuevoGasto" 
      alt=""
      @click="mostrarModal"
      >
    </div>

    <Modal 
    v-if="modal.mostrar"
    @ocultar-modal="ocultarModal"

    :gasto="gasto"
      v-model:nombre="gasto.nombre"
              v-model:cantidad="gasto.cantidad"
              v-model:categoria="gasto.categoria"
    />
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
