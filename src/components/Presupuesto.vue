<script setup>
import { ref } from 'vue'
import Alerta from './Alerta.vue';

const presupuesto = ref()
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
    <div class="presupuesto-home">
        <h2>Iniciemos estableciendo un presupuesto</h2>
        <Alerta v-if="error">
            {{ error }}
        </Alerta>
        <form
            @submit.prevent="definirPresupuesto"
        >
            <!-- <label for="nuevo-presupuesto">Nuevo presupuesto</label> -->
            <input 
            id="nuevo-presupuesto"
            placeholder="Añade un presupuesto"
            type="number"
            min="0"
            v-model.number="presupuesto"
            >
            <input type="submit" value="Nuevo presupueto">
        </form>
    </div>
</template>

<style lang="scss" scoped >
form{
    display: flex;
    width: 100%;
    align-items: center;
    flex-flow: column;
    padding:  0px;
    margin: 0;
}
.presupuesto-home{
    height: 100%;
    width: 90%;
    padding: 0 5%;
    display: flex;
    flex-flow: column;
    justify-content: center;
}
h2{
    margin: 0;
}

</style>
