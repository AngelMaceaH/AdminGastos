<template>
  <form class="presupuesto" @submit.prevent="definirPresupuesto">
    <Alerta v-if="error">
      {{ error }}
    </Alerta>
    <div class="campo">
      <label for="">Definir Presupuesto</label>
      <input
        type="number"
        id="nuevo-presupuesto"
        class="nuevo-presupuesto"
        placeholder="Añade tu presupuesto"
        v-model.number="presupuesto"
      />

      <input
        type="submit"
        value="Definir Presupuesto"
        class="submit-presupuesto"
      />
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
import Alerta from "./Alerta.vue";
const presupuesto = ref(0);
const error = ref("");
const emit = defineEmits(["definir-presupuesto"]);

const definirPresupuesto = () => {
  if (presupuesto.value <= 0) {
    error.value = "El presupuesto no puede ser 0 o negativo";
    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }
  emit("definir-presupuesto", presupuesto.value);
};
</script>

<style scoped>
.presupuesto {
  width: 100%;
}

.campo {
  display: grid;
  gap: 2rem;
}

.presupuesto label {
  font-size: 2.8rem;
  text-align: center;
  color: var(--azul);
}

.presupuesto input[type="number"] {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
  text-align: center;
}

.presupuesto input[type="submit"] {
  background-color: var(--azul);
  border: none;
  padding: 1rem;
  text-align: center;
  font-size: 2rem;
  margin-top: 2rem;
  color: var(--blanco);
  font-weight: 900;
  width: 100%;
  transition: background-color 0.3s ease;
}

.presupuesto input[type="submit"]:hover {
  cursor: pointer;
  background-color: #1048a4;
}
</style>
