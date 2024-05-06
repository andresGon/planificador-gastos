<script setup>
import { ref, computed } from 'vue'

import Alerta from './Alerta.vue';
import cerrarModal from '../assets/img/cerrar.svg'


const error = ref('')
const emit = defineEmits(['ocultar-modal','guardar-gasto','update:nombre', 'update:cantidad', 'update:categoria', 'eliminar-gasto'])

const props = defineProps({
    modal: {
            type: Object,
            required: true
        },
        nombre: {
            type: String,
            required: true,
        },
        cantidad: {
            type: [String, Number],
            required: true,
        },
        categoria: {
            type: String,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        id: {
            type: [String, null],
            required: true
        }
})
const old = props.cantidad
const agregarGasto = () => {
    const { nombre, cantidad, categoria, disponible, id } = props
        if([nombre, cantidad, categoria].includes('')) {
            error.value = 'Todos los campos son obligatorios'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }

        // Validar la cantidad
        if(cantidad <= 0) {
            error.value = 'Cantidad no válida'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }
        // validar que se no gaste mas de lo disponible
        if(id){
            // tomar en cuenta el gasto ya realizado
            if(cantidad > old + disponible){
                error.value = 'has excedido el presupuesto'
                setTimeout(() => {
                    error.value = ''
                }, 3000);
                return
            }
        }else{
            if(cantidad > disponible ) {
                error.value = 'has excedido el presupuesto'
                setTimeout(() => {
                    error.value = ''
                }, 3000);
                return
            }
        }
        


        emit('guardar-gasto')

}
const isEditing = computed (()=>{
    return props.id
})
</script>
<template>
    <div class="modal">
        

        <div class="modal-inner">
            <button 
                type="button" 
                class="btn-eliminar"
                v-if="isEditing"
                @click="$emit('eliminar-gasto')"
                >
                
                <span>
                    Eliminar Gasto
                </span>
                
            </button>

            <div class="modal-cerrar" @click="$emit('ocultar-modal')">
                <svg 
                    id="Capa_1" 
                    data-name="Capa 1" 
                    xmlns="http://www.w3.org/2000/svg" 
                    viewBox="0 0 259.43 259.21">
                    <defs>
                    </defs>
                    <rect class="cls-1" x="165" y="52" width="71" height="295" transform="translate(129.32 -153.04) rotate(45)"/>
                    <rect class="cls-1" x="164.37" y="51.59" width="71" height="295" transform="translate(-152.71 129.95) rotate(-45)"/>
                </svg>
            </div> 
             
        <!-- <p>{{ isEditing ? 'Guardar Cambios' : 'Añadir gasto' }}</p> -->
        

        <form  @submit.prevent="agregarGasto">
            <Alerta v-if="error">{{error}}</Alerta>
            <div class="campo">
                <label for="nombre-gasto">Nombre gasto:</label>
                <input 
                    id="nombre-gasto" 
                    type="text"
                    placeholder="Añade el Nombre del Gasto"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                >
            </div>


            <div class="campo">
                <label for="cantidad">Cantidad:</label>
                <input  type="number"
                        id="cantidad"
                        placeholder="Añade la cantidad"
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"
                        >
            </div>

            <div class="campo">
                <label for="categoria">Categoría:</label>
                <select 
                id="categoria"
                :value="categoria"
                @input="$emit('update:categoria', $event.target.value)"
                >
                <option value="">-- Seleccione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos Varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
            </select>
            </div>
            <input 
                type="submit"
                :value="[isEditing ? 'Guardar Cambios' : 'Añadir Gasto' ]"
                >
        </form>
        
    </div>
    </div>
</template>

<style lang="scss" scoped>
.modal{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0,0,0,0.4);
    display: flex;
    flex-flow: column;
    align-items: center;
    form{
        display: flex;
        flex-flow: column;
        max-width: 500px;
    }
    p{
        color: #fff;
    }
    &-inner{
        position: absolute;
        bottom: 0;
        left: 0;
        width: calc(100% - 40px);
        padding: 20px 5%;
        background: var(--second-color);
        border-top-left-radius: 28px;
        border-top-right-radius: 28px;
    }
    &-cerrar{
        width: 40px;
        height: 40px;
        border-radius: 50%;
        position: absolute;
        top: -15%;
        left: 50%;
        transform: translateX(-50%);
        background: var(--main-color);
        display: flex;
        align-items: center;
        justify-content: center;
        svg{
            width: 50%;
        }
    }
}
label{
    margin: 8px 0;
    display: block;
    width: 100%;
    text-align: left;
    font-size: 0.8rem;
    color: var(--main-color);
}
select{
    margin: 0 0 20px 0;
    width: 100%;
}

.cls-1{
    fill:var(--second-color);
}
.btn-eliminar{
    background: none;
    color: var(--main-color);
    border: none;
    font-size: .8rem;
    display: flex;
    margin: auto;
    margin-right: 0;
    text-decoration: underline;
    svg{
        margin-top: -2px;
    }
}
.icon-trash{
    path{
        fill: var(--main-color);
    }
}
</style>