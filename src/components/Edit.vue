<template>
  <div id="counter_modal">
    <div id="counter_head">
      <h2>Editar Tarea</h2>
      <button @click="closeModal" id="close">
        <i class="fa-solid fa-x"></i>
      </button>
    </div>
    <div>
      <form @submit.prevent>
        <h2>index: {{ editList.index }}</h2>
        <h3>Tarea</h3>
        <textarea v-model.trim="editList.tarea" cols="30" rows="2">{{
          editList.tarea
        }}</textarea>

        <h3>Estado</h3>
        <select v-model.trim="editList.estado">
          <option value="Empezar">Por empezar</option>
          <option value="Curso">En curso</option>
          <option value="Listo">Listo</option>
        </select>

        <h3>Prioridad</h3>
        <select v-model.trim="editList.prioridad">
          <option value="Alta">Alta</option>
          <option value="Media">Media</option>
          <option value="Baja">Baja</option>
        </select>

        <h3>Fecha</h3>
        <input v-model.trim="editList.fecha" type="date" />
      </form>
    </div>
    <div>
      <button @click="editTask">Actualizar</button>
      <button @click="closeModal" id="delete">Cancelar</button>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  props: {
    editList: Object,
  },
  methods: {
    editTask() {
      if (
        this.editList.tarea === "" ||
        this.editList.estado === "" ||
        this.editList.prioridad === "" ||
        this.editList.fecha === ""
      ) {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: "Por favor, completa todos los campos.",
          confirmButtonColor: "#52a575",
        });
      } else {
        const task = {
          index: this.editList.index,
          tarea: this.editList.tarea,
          estado: this.editList.estado,
          prioridad: this.editList.prioridad,
          fecha: this.editList.fecha,
        };
        this.$emit("editTask", task);
      }
    },
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>

<style></style>
