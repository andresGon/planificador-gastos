<script setup>
import { computed } from 'vue';
import CircleProgress from 'vue3-circle-progress'
import "vue3-circle-progress/dist/circle-progress.css"
import { formatearCantidad } from '../../helpers';

defineEmits(['reset-app'])
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

const porcentaje = computed(()=>{
    return parseInt(((props.presupuesto - props.disponible)/props.presupuesto) * 100  ) 
})
</script>

<template>
    <div class="control-presupuesto">
        <p>Control de presupuesto</p>
        {{ porcentaje }} %
        <CircleProgress 
            :percent="porcentaje"
            :size="250"
            :border-width="20"
            :border-bg-width="20"
            fill-color="red"
            empty-color="pink"
        />
        <button 
        class="reset-app"
        type="button"
        @click="$emit('reset-app')"
        >
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