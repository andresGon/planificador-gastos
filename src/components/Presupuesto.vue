<script setup>
import { ref } from 'vue'
import Alerta from './Alerta.vue';

const presupuesto = ref(0)
const error = ref('')

const emit = defineEmits(['definir-presupuesto'])

const definirPresupuesto = ()=>{
    if(presupuesto.value <= 0 || presupuesto.value === ''){
        error.value = 'Presupuesto no válido'
        setTimeout(() => {
            error.value = ''
        }, 3000);
        return
    }
    emit('definir-presupuesto', presupuesto.value)
}
</script>
<template>
    <h2>Inicia con un presupuesto</h2>
    <Alerta v-if="error">
        {{ error }}
    </Alerta>
    <form
        @submit.prevent="definirPresupuesto"
    >
        <label for="nuevo-presupuesto">Indica un presupuesto</label>
        <input 
        id="nuevo-presupuesto"
        placeholder="Añade un presupuesto"
        type="number"
        min="0"
        v-model.number="presupuesto"
        >
        <input type="submit" value="Establecer presupueto">
    </form>
</template>

<style lang="scss" scoped >
form{
    display: flex;
    width: 100%;
    align-items: center;
    flex-flow: column;
    padding: 10px;
    border: 1px solid #ccc;
}
</style>
