<template>
  <div class="gasto sombra">
    <div class="contenido">
      <div class="detalles">
        <p class="categoria">{{ gasto.categoria }}</p>
        <p class="nombre">{{ gasto.nombre }}</p>
        <p class="nombre">{{ gasto.nombre }}</p>
        <p class="fecha">{{ gasto.fecha }}</p>
      </div>
    </div>
    <p class="cantidad">{{ formatearCantidad(gasto.cantidad) }}</p>
  </div>
</template>

<script setup>
import { formatearCantidad } from "../helpers";
const props = defineProps({
  gasto: {
    type: Object,
    required: true,
  },
});
</script>

<style lang="scss" scoped></style>
