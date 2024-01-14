<template>
  <div id="counter_table">
    <div id="counter">
      <h1>Gestión de Tareas</h1>
      <div>
        <button @click="toggleAdd">
          <i id="plus" class="fa-solid fa-square-plus"></i> Agregar Tarea
        </button>
      </div>
      <div>
        <table class="tabla">
          <thead>
            <tr>
              <th>Tarea</th>
              <th>Estado</th>
              <th>Prioridad</th>
              <th>Fecha</th>
              <th>Editar/Eliminar</th>
            </tr>
          </thead>
          <tbody v-for="(lista, index) in list" :key="index">
            <tr>
              <td>{{ lista.tarea }}</td>
              <td
                :style="{
                  color: 'white',
                  backgroundColor:
                    lista.estado === 'Empezar'
                      ? '#b92c2c'
                      : lista.estado === 'Curso'
                      ? '#eca842'
                      : lista.estado === 'Listo'
                      ? '#52a575'
                      : 'transparent',
                }"
              >
                <!-- Ajusté la lógica para mostrar el ícono según el estado -->
                <i
                  v-if="lista.estado === 'Empezar'"
                  id="start"
                  class="fa-solid fa-hourglass-start"
                ></i>
                <i
                  v-else-if="lista.estado === 'Curso'"
                  id="progress"
                  class="fa-solid fa-circle-half-stroke"
                ></i>
                <i v-else id="end" class="fa-solid fa-hourglass-end"></i>
                {{ lista.estado }}
              </td>
              <td
                :style="{
                  color: 'white',
                  backgroundColor:
                    lista.prioridad === 'Baja'
                      ? '#52a575'
                      : lista.prioridad === 'Media'
                      ? '#eca842'
                      : lista.prioridad === 'Alta'
                      ? '#b92c2c'
                      : 'transparent',
                }"
              >
                {{ lista.prioridad }}
              </td>
              <td>{{ lista.fecha }}</td>
              <td>
                <button @click="toggleEdit(index, lista)" id="edit">
                  <i class="fa-solid fa-pen-to-square"></i>
                </button>

                <button id="trash" @click="deleteTask(index)">
                  <i class="fa-solid fa-trash-can"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <transition name="fade">
      <Add
        v-show="add"
        @addTask="addTask"
        @close="toggleAdd"
        :task="editList"
      />
    </transition>
    <transition name="fade">
      <Edit
        v-show="edit"
        @editTask="editTask"
        @close="toggleEdit"
        :editList="editList"
      />
    </transition>
  </div>
</template>

<script>
import Edit from "./components/Edit.vue";
import Add from "./components/Add.vue";
import Swal from "sweetalert2";

export default {
  components: { Edit, Add },
  data() {
    return {
      edit: false,
      add: false,
      list: [],
      editList: {
        index: null,
        tarea: "",
        estado: null,
        prioridad: null,
        fecha: null,
      },
    };
  },
  methods: {
    saveToLocalStorage() {
      try {
        localStorage.setItem("tareasList", JSON.stringify(this.list));
      } catch (error) {
        console.error("Error al guardar en localStorage:", error);
      }
    },

    loadFromLocalStorage() {
      try {
        const storedList = localStorage.getItem("tareasList");
        if (storedList) {
          this.list = JSON.parse(storedList);
        }
      } catch (error) {
        console.error("Error al cargar desde localStorage:", error);
      }
    },
    toggleEdit(index, task) {
      this.editList = { index, ...task };
      this.edit = !this.edit;
    },
    toggleAdd() {
      this.add = !this.add;
    },
    addTask(newTask) {
      this.list.push(newTask);
      this.toggleAdd();
      this.saveToLocalStorage();
      Swal.fire({
        position: "center",
        icon: "success",
        text: "Tarea Guardada",
        showConfirmButton: false,
        timer: 1500,
      });
    },
    editTask(task) {
      this.list.splice(this.editList.index, 1, task);
      this.toggleEdit();
      this.saveToLocalStorage();
      Swal.fire({
        position: "center",
        icon: "success",
        text: "Tarea Actualizada",
        showConfirmButton: false,
        timer: 1500,
      });
    },
    deleteTask(index) {
      Swal.fire({
        text: "¿Seguro quieres eliminar la tarea?",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#52a575",
        cancelButtonColor: "#b92c2c",
        cancelButtonText: "Cancelar",
        confirmButtonText: "Si, eliminar tarea!",
      }).then((result) => {
        if (result.isConfirmed) {
          this.list.splice(index, 1);
          this.saveToLocalStorage();
          Swal.fire({
            position: "center",
            icon: "success",
            text: "Tarea eliminada",
            showConfirmButton: false,
            timer: 1500,
          });
        }
      });
    },
  },
  created() {
    this.loadFromLocalStorage();
  },
};
</script>

<style>
.fade-enter-active {
  transition: opacity 2s ease;
}
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
