<script setup>
import { ref, reactive, watch } from 'vue'
import { generarId } from '../helpers';
import Presupuesto from './components/Presupuesto.vue';
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg';
import Modal from './components/Modal.vue';
import Gasto from './components/Gasto.vue';


const modal = reactive({
  mostrar:false,
  animar:false
})

const presupuesto =ref(0)
const disponible = ref(0)
const gastado = ref(0)

const gasto = reactive({
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now()
  })

const gastos = ref([])
watch(gastos, ()=>{
  const totalGastado = gastos.value.reduce((total, gasto)=> gasto.cantidad + total, 0)
  gastado.value = totalGastado
  disponible.value = presupuesto.value - totalGastado
},{
  deep:true
})

const definirPresupuesto = (cantidad) => {
  presupuesto.value=cantidad
  disponible.value = cantidad
}

const mostrarModal = ()=>{
  modal.mostrar = true
  modal.animar = true
}

const ocultarModal = ()=>{
  modal.mostrar = false
  modal.animar = false
}



const guardarGasto = () => {
   gastos.value.push({
    ...gasto,
    id:generarId()
   })
    ocultarModal()
    // reiniciar objeto
   Object.assign(gasto, {
    nombre: '',
    cantidad: '',
    categoria: '',
    id: null,
    fecha: Date.now()
   })

  }


  
</script>

<template>
  <div class="container" :class="{fijar: modal.mostrar}">
    <h1>Planificador de gastos</h1>
    <Presupuesto
      v-if="presupuesto === 0"
      @definir-presupuesto="definirPresupuesto"
    />
    <ControlPresupuesto
      v-else
      :presupuesto="presupuesto"
      :disponible="disponible"
      :gastado="gastado"
    />
    <div v-if="presupuesto > 0">
      <div class="listado-gastos">
        <h2> {{ gastos.length > 0 ? 'Gastos' : 'No hay gastos' }}</h2>
        <Gasto
          v-for="gasto in gastos"
          :key="gasto.id"
          :gasto="gasto"
        />

      </div>
      <div class="crear-gasto" >
        <img :src="iconoNuevoGasto" 
        alt=""
        @click="mostrarModal"
        >
      </div>
    </div>
    

    <Modal 
    v-if="modal.mostrar"
    @ocultar-modal="ocultarModal"
    @guardar-gasto="guardarGasto"
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
.listado-gastos{
  display: flex;
  flex-flow: column;
  align-items: center;
}
</style>
