<script setup>
import { formatearCantidad } from '../../helpers';
const props = defineProps({
    presupuesto:{
        type: Number,
        required:true
    },
    disponible:{
        type:Number,
        required:true
    },
    gastado:{
        type:Number,
        required:true
    }
})
</script>

<template>
    <div class="control-presupuesto">
        <p>Control de presupuesto</p>
        <img src="../assets/img/icono_gastos.svg" alt="">
        <button class="reset-app">
            Resetar app
        </button>

        <p>Presupuesto {{ formatearCantidad(presupuesto)}}</p>
        <p>Disponible $ {{ formatearCantidad(disponible) }}</p>
        <p>Gastado  {{ formatearCantidad(gastado) }}</p>
    </div>
    
</template>

<style scoped lang="scss">
    img{
        width: 100px;
    }
    .control-presupuesto{
        border: 1px solid red;
    }
</style>