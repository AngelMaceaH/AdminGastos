<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img :src="cerrarModal" @click="$emit('ocultar-modal')" />
    </div>
    <div
      class="contenedor contenedor-formulario"
      :class="[modal.animar ? 'animar' : 'cerrar']"
    >
      <form action="" class="nuevo-gasto" @submit.prevent="agregarGasto">
        <legend>
          {{ isEditing ? "Actualizar un gasto" : "Añadir gasto" }}
        </legend>
        <Alerta v-if="error">{{ error }}</Alerta>
        <div class="campo">
          <label for="nombre">Nombre Gasto:</label>
          <input
            type="text"
            id="nombre"
            placeholder="Añade el nombre del gasto"
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="cantidad">Cantidad:</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Añade la cantidad del Gasto, ej. 300"
            :value="cantidad"
            @input="$emit('update:cantidad', +$event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="categoria">Categoría:</label>
          <select
            name=""
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
          :value="[isEditing ? 'Guardar Cambios' : 'Añadir Gasto']"
        />
      </form>
      <button
        type="button"
        class="btn-eliminar"
        v-if="isEditing"
        @click="$emit('eliminar-gasto')"
      >
        Eliminar Gasto
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Alerta from "./Alerta.vue";
import cerrarModal from "../assets/img/cerrar.svg";
const error = ref("");
const emit = defineEmits([
  "ocultar-modal",
  "guardar-gasto",
  "eliminar-gasto",
  "update:nombre",
  "update:cantidad",
  "update:categoria",
]);
const props = defineProps({
  modal: {
    type: Object,
    required: true,
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
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
  id: {
    type: [String, null],
    required: true,
  },
});
const old = props.cantidad;
const agregarGasto = () => {
  const { cantidad, categoria, nombre, disponible, id } = props;
  if ([nombre, cantidad, categoria].includes("")) {
    error.value = "Todos los campos son obligatorios";
    return;
  }
  if (cantidad <= 0) {
    error.value = "La cantidad no es valida";
    return;
  }
  if (id) {
    if (cantidad > old + disponible) {
      error.value = "Haz excedido el presupuesto";
      return;
    }
  } else {
    if (cantidad > disponible) {
      error.value = "Haz excedido el presupuesto";
      return;
    }
  }

  emit("guardar-gasto");
  setTimeout(() => {
    error.value = "";
  }, 3000);
};
const isEditing = computed(() => {
  return props.id;
});
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

.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}

.animar {
  opacity: 1;
}

.cerrar {
  opacity: 0;
}

.nuevo-gasto {
  margin: 10rem auto 0 auto;
  display: grid;
  gap: 2rem;
}

.nuevo-gasto legend {
  text-align: center;
  color: var(--blanco);
  font-size: 3rem;
  font-weight: 700;
}

.campo {
  display: grid;
  gap: 2rem;
}

.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
}

.nuevo-gasto label {
  color: var(--blanco);
  font-size: 3rem;
}

.nuevo-gasto input[type="submit"] {
  background-color: var(--azul);
  color: var(--blanco);
  font-weight: 700;
  cursor: pointer;
}
.btn-eliminar {
  border: none;
  padding: 1rem;
  width: 100%;
  background-color: #ef4444;
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--blanco);
  margin-top: 10rem;
  cursor: pointer;
}
</style>
