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
        <p>{{ isEditing ? 'Guardar Cambios' : 'Añadir gasto' }}</p>
        <div class="cerrar-modal"
        
        >
            <img
                :src="cerrarModal"
                @click="$emit('ocultar-modal')"
            />
        </div>

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
                        placeholder="Añade la cantidad del Gasto, ej. 300"
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
        <button 
            type="button" 
            class="btn-eliminar"
            v-if="isEditing"
            @click="$emit('eliminar-gasto')"
            >
            Eliminar gasto
        </button>
    </div>
</template>

<style lang="scss" scoped>
.modal{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0,0,0,0.8);
    display: flex;
    flex-flow: column;
    align-items: center;
    form{
        display: flex;
        flex-flow: column;
        max-width: 500px;
    }
    p, label{
        color: #fff;
    }
}
.cerrar-modal{
    width: 20px;
}
</style>