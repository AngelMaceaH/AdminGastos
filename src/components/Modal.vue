<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img :src="cerrarModal" @click="$emit('ocultar-modal')">
        </div>
        <div class="contenedor">
            <form action="">
                <legend>Añadir Gasto</legend>

            </form>
        </div>
    </div>
</template>

<script setup>
import cerrarModal from '../assets/img/cerrar.svg'
const emit = defineEmits(['ocultar-modal'])

</script>

<style scoped>
.modal {
    position: absolute;
    background-color: rgb(0 0 0 / 0.9);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}

.cerrar-modal {
    position: absolute;
    right: 3rem;
    top: 3rem;
}

.cerrar-modal img {
    width: 3rem;
    cursor: pointer;
}
</style>