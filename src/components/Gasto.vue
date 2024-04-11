<script setup>
import { formatearCantidad, formatearFecha } from '../../helpers';

import IconoAhorro from '../assets/img/icono_ahorro.svg'
    import IconoCasa from '../assets/img/icono_casa.svg'
    import IconoComida from '../assets/img/icono_comida.svg'
    import IconoGastos from '../assets/img/icono_gastos.svg'
    import IconoOcio from '../assets/img/icono_ocio.svg'
    import IconoSalud from '../assets/img/icono_salud.svg'
    import IconoSuscripciones from '../assets/img/icono_suscripciones.svg'


    const diccionarioIconos = {
        ahorro : IconoAhorro,
        comida : IconoComida,
        casa : IconoCasa,
        gastos : IconoGastos,
        ocio : IconoOcio,
        salud : IconoSalud,
        suscripciones : IconoSuscripciones
    }

const props = defineProps({
    gasto:{
        type:Object,
        required:true
    }
})

defineEmits(['seleccionar-gasto'])
</script>
<template>
    <div class="gasto sombra" @click="$emit('seleccionar-gasto', gasto.id)">
        <div class="contenido">
            <img 
                :src="diccionarioIconos[gasto.categoria]" 
                alt="icono gasto"
                class="icono"
            >
            <div class="detalles">
                <p class="nombre"
                
                > {{ gasto.nombre }}</p>
                <p class="categoria">{{ gasto.categoria}}</p>
                <p class="fecha"> <span></span> {{  formatearFecha(gasto.fecha) }}</p>
            </div>
        </div>
        <p class="cantidad"> {{ formatearCantidad(gasto.cantidad) }}</p>
    </div>
</template>

<style lang="scss" scoped>
.icono{
    width: 60px;
}
.gasto{
    display: flex;
    justify-content: space-between;
    width: 80%;
    color: var(--main-color);
    box-shadow: 4px 4px 0px var(--main-color);
    border-radius: 16px;
    padding: 16px 5%;
}
.contenido{
    display: flex;
}
.detalles{
    display: flex;
    flex-flow: column;
    text-align: left;
    margin: 0 10px;
    p{
        margin: 0;
    }
}
.nombre{
    cursor: pointer;
    font-weight: bold;
}
</style>