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
        <div class="item">
                <div class="col">
                        <CircleProgress 
                        :percent="porcentaje"
                        :size="180"
                        :border-width="20"
                        :border-bg-width="20"
                        fill-color="#000"
                        empty-color="#FF6A6A"
                    />
                    <span>{{ porcentaje }}%</span>
                </div>
                <div class="col">
                    <p>
                        <b>Presupuesto</b>  
                        <b>{{ formatearCantidad(presupuesto)}}</b>    
                    </p>
                    <button 
                            class="reset-app"
                            type="button"
                            @click="$emit('reset-app')"
                            >
                                Reset
                    </button>
                </div>
        </div>
            
        <div class="item">
            <div class="item-value">
                <div>Disponible </div>
                <div>{{ formatearCantidad(disponible) }}</div>
            </div>
        </div>
        <div class="item">
            <div class="item-value">
                <div>Gastado</div>
                 <div>{{ formatearCantidad(gastado) }}</div>
            </div>
        </div>
        
    </div>
    
</template>

<style scoped lang="scss">
    img{
        width: 100px;
    }
    .control-presupuesto{
        width: 90%;
        margin: 20px auto;
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        grid-template-rows: 226px 112px;
        gap: 10px;
        .item{
            border-radius: 18px;
            p{
                margin: 0;
            }
            &:nth-child(1){
                background-color: var(--main-color);
                color: var(--second-color);
                border: 3px solid var(--main-color);
                grid-column: 1/3;
                display: flex;
                padding: 8px;
                gap: 8px;
                span{
                    position: absolute;
                    top: 50%;
                    left: 50%;
                    transform: translate(-50%, -50%);
                    font-size: 2rem;
                    font-weight: bold;
                }
                .col{
                    height: 100%;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    position: relative;
                    &:nth-child(2){
                        display: flex;
                        flex-flow: column;
                        justify-content: center;
                        p{
                            margin: 0;
                            text-align: right;
                            b{
                                display: block;
                                &:nth-child(2){
                                    font-size: 2rem;
                                }
                            }
                        }
                        button{
                            background: none;
                            border: none;
                            color: var(--second-color);
                            position: absolute;
                            bottom: 0;
                            right: 0;
                            font-weight: bold;
                            font-size: .8rem;
                            text-decoration: underline;
                        }
                    }
                }
            }
            &:nth-child(2){
                background-color: var(--second-color);
                color: var(--main-color);
                border: 3px solid var(--main-color);
                display: flex;
                align-items: center;
                justify-content: center;
            }
            &:nth-child(3){
                background-color: var(--main-color);
                color: var(--second-color);
                display: flex;
                align-items: center;
                justify-content: center;
            }
            .item-value{
                div{
                    display: block;
                    font-weight: bold;
                    &:nth-child(2){
                        font-size: 1.6rem;
                    }
                }
            }
        }
    }
</style>