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

watch(modal, () =>{
  if(!modal.mostrar){
    // reiniciar objeto
    reiniciarStateGasto()
  }
}, {
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
  if(gasto.id){
    // editando
    const { id } = gasto
    const i = gastos.value.findIndex((gasto => gasto.id === id))
    gastos.value[i] = {...gasto}
  }else{
    // registro
    gastos.value.push({
    ...gasto,
    id:generarId()
   })
  }
    ocultarModal()
   reiniciarStateGasto()
  }

  const reiniciarStateGasto = () => {
     // reiniciar objeto
      Object.assign(gasto, {
        nombre: '',
        cantidad: '',
        categoria: '',
        id: null,
        fecha: Date.now()
      })
  }

  const seleccionarGasto = id => {
     const gastoEditar = gastos.value.filter(gasto => gasto.id === id)[0]
     Object.assign(gasto, gastoEditar)
     mostrarModal()
  }

  const eliminarGasto = () =>{
    if(confirm('Eliminar?')){
      gastos.value = gastos.value.filter(gastoState => gastoState.id !== gasto.id)
      ocultarModal()
    }
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
          @seleccionar-gasto="seleccionarGasto"
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
    @eliminar-gasto="eliminarGasto"
    :modal="modal"
    :disponible="disponible"
    :id="gasto.id"
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
