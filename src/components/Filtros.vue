<script setup>
defineProps(['filtro'])
defineEmits(['update:filtro'])
</script>

<template>
    <div class="filtros">
        <form>
            <div class="campo">
                <label for="filtro">Filtrar Gastos</label>

                <select 
                    id="filtro"
                    :value="filtro"
                    @input="$emit('update:filtro', $event.target.value)"
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
        </form>
    </div>
</template>

<style lang="scss" scoped>
        select{
            display: block;
            margin: auto;
        }
</style>