<template>
  <div id="counter_modal">
    <div id="counter_head">
      <h2>Agregar Tarea</h2>
      <button @click="closeModal" id="close">
        <i class="fa-solid fa-x"></i>
      </button>
    </div>
    <div>
      <form @submit.prevent>
        <h3>Tarea</h3>
        <textarea v-model="newList.tarea" cols="30" rows="2"></textarea>

        <h3>Estado</h3>
        <select v-model="newList.estado">
          <option value="Empezar">Por empezar</option>
          <option value="Curso">En curso</option>
          <option value="Listo">Listo</option>
        </select>

        <h3>Prioridad</h3>
        <select v-model="newList.prioridad">
          <option value="Alta">Alta</option>
          <option value="Media">Media</option>
          <option value="Baja">Baja</option>
        </select>

        <h3>Fecha</h3>
        <input v-model="newList.fecha" type="date" />
      </form>
    </div>
    <div>
      <button @click="saveList">Guardar</button>
      <button @click="closeModal" id="delete">Cancelar</button>
    </div>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  data() {
    return {
      newList: {
        tarea: "",
        estado: "",
        prioridad: "",
        fecha: "",
      },
    };
  },
  methods: {
    saveList() {
      if (
        this.newList.tarea === "" ||
        this.newList.estado === "" ||
        this.newList.prioridad === "" ||
        this.newList.fecha === ""
      ) {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: "Por favor, completa todos los campos.",
          confirmButtonColor: "#52a575",
        });
      } else {
        const newTask = {
          tarea: this.newList.tarea,
          estado: this.newList.estado,
          prioridad: this.newList.prioridad,
          fecha: this.newList.fecha,
        };
        this.$emit("addTask", newTask);
        this.newList.tarea = "";
        this.newList.estado = "";
        this.newList.prioridad = "";
        this.newList.fecha = "";
      }
    },
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>

<style></style>
