<script setup>
import { ref, reactive, watch, computed, onMounted } from 'vue'
import { generarId } from '../helpers';
import Presupuesto from './components/Presupuesto.vue';
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg';
import Filtros from './components/Filtros.vue';
import Modal from './components/Modal.vue';
import Gasto from './components/Gasto.vue';



const modal = reactive({
  mostrar:false,
  animar:false
})

const presupuesto =ref(0)
const disponible = ref(0)
const gastado = ref(0)
const filtro = ref('')

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
  localStorage.setItem('gastos', JSON.stringify(gastos.value))
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

watch(presupuesto,()=>{
  localStorage.setItem('presupuesto', presupuesto.value)
})


onMounted(()=>{
  const presupuestoStorage = localStorage.getItem('presupuesto')
  if(presupuestoStorage){
    presupuesto.value = Number(presupuestoStorage)
    disponible.value = Number(presupuestoStorage)
  }

  const gastosStorage = localStorage.getItem('gastos')
  if(gastosStorage){
    gastos.value = JSON.parse(gastosStorage)
  }
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

  const gastosFiltrados = computed(()=>{
    if(filtro.value){
      return gastos.value.filter(gasto => gasto.categoria === filtro.value)
    }
    return gastos.value
  })

  const resetApp = () =>{
    if(confirm('¿Deseas reiniciar presupuesto y gastos?')){
      gastos.value=[]
      presupuesto.value=0
    }
  }
</script>

<template>
  <div class="container" :class="{fijar: modal.mostrar}">
    <Presupuesto
      v-if="presupuesto === 0"
      @definir-presupuesto="definirPresupuesto"
    />
    <ControlPresupuesto
      v-else
      :presupuesto="presupuesto"
      :disponible="disponible"
      :gastado="gastado"
      @reset-app ="resetApp"
    />
    <div v-if="presupuesto > 0">
      <Filtros
        v-model:filtro="filtro"
      />
      <div class="listado-gastos">
        <h2> {{ gastosFiltrados.length > 0 ? 'Gastos' : 'No hay gastos' }}</h2>
        <Gasto
          v-for="gasto in gastosFiltrados"
          :key="gasto.id"
          :gasto="gasto"
          @seleccionar-gasto="seleccionarGasto"
        />

      </div>
      <div class="crear-gasto" @click="mostrarModal"> Nuevo Gasto
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

<style lang="scss">

:root{
    --main-color:#00ff7f;
    --second-color:#5e0447;
}
.crear-gasto{
  width: 24px;
}
.listado-gastos{
  display: flex;
  flex-flow: column;
  align-items: center;
}


html{
    background: var(--second-color);
    font-family: "Inter", sans-serif; 
}

h2{
    color: var(--main-color);   
}

.container{
    height: 100vh;
    overflow: auto;
    text-align: center;
    width: 100%;
    position: relative;
}

.presupuesto-home{
    width: 100%;
    height: 100%; 
    form{
      margin-top: 16px;
      gap: 16px;
    }
}

input[type=number]{
    border: 3px solid var(--main-color);
    width: 100%;
    height: 50px;
    border-radius: 18px;
    text-align: center;
    background: transparent;
    color: var(--main-color);
    font-size: 22px;
    outline: none;
}

input[type=number]::placeholder {
color: var(--main-color);
opacity: .6; /* Firefox */
}


input[type=submit], .crear-gasto{
    width: 100%;
    height: 50px;
    border: none;
    border-radius: 18px;
    text-align: center;
    background: var(--main-color);
    color: var(--second-color);
    font-size: 22px;
    outline: none;
    font-size: 18px;
    font-weight: bold;
}
.crear-gasto{
  display: flex;
  align-items: center;
  justify-content: center;
  width: 90%;
  position: absolute;
  bottom: 5%;
  left: 5%;
}
</style>
