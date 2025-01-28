<template>
  <div class="user-list">
    <div class="d-flex justify-content-between align-items-center mt-5 mb-5 top-bar">
      <h1 class="text-2xl font-bold mb-4">Lista de Usuarios</h1>
      <input v-model="searchTerm" type="text" placeholder="Buscar usuarios..." class="border p-2 rounded mb-4 w-full" />
      <button class="btn btn-primary" style="margin-left: 70px;" @click="openModal('crear')">
        Crear Usuario
      </button>
    </div>
    <table class="table-auto w-full border-collapse border border-gray-300">
      <thead>
        <tr>
          <th class="border px-4 py-2">ID</th>
          <th class="border px-4 py-2">Nombre</th>
          <th class="border px-4 py-2">Correo</th>
          <th class="border px-4 py-2">Edad</th>
          <th class="border px-4 py-2">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in filteredUsers" :key="user.id">
          <td class="border px-4 py-2">{{ user.id }}</td>
          <td class="border px-4 py-2">{{ user.nombre }}</td>
          <td class="border px-4 py-2">{{ user.correo }}</td>
          <td class="border px-4 py-2">{{ user.edad }}</td>
          <td class="border px-4 py-2 d-flex justify-content-between align-items-center">
            <button class="btn btn-primary me-2" @click="openModal('editar', user)">
              Editar
            </button>
            <button @click="deleteUser(user.id)" class="btn btn-danger">
              Eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Paginación -->
    <div class="mt-4 flex justify-center">
      <button @click="changePage(currentPage - 1)" :disabled="currentPage === 1" class="px-4 py-2 border rounded-l">
        Anterior
      </button>
      <span class="px-4 py-2 border">{{ currentPage }}</span>
      <button @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages"
        class="px-4 py-2 border rounded-r">
        Siguiente
      </button>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="userModal" tabindex="-1" aria-labelledby="userModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="userModalLabel">{{ modalTitle }}</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="handleSubmit">
              <div class="mb-3">
                <label for="nombre" class="form-label">Nombre</label>
                <input v-model="form.nombre" type="text" class="form-control" id="nombre" required />
              </div>
              <div class="mb-3">
                <label for="correo" class="form-label">Correo</label>
                <input v-model="form.correo" type="email" class="form-control" id="correo" required />
              </div>
              <div class="mb-3">
                <label for="edad" class="form-label">Edad</label>
                <input v-model="form.edad" type="number" class="form-control" id="edad" required />
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                <button type="submit" class="btn btn-primary">{{ modalAction }}</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import api from "@/services/api";
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap/dist/js/bootstrap.js';
import { Modal } from 'bootstrap';

export default {
  name: "UserList",
  data() {
    return {
      users: [],
      searchTerm: "",
      currentPage: 1,
      itemsPerPage: 5,
      modalTitle: "",
      modalAction: "",
      form: {
        id: null,
        nombre: "",
        correo: "",
        edad: null,
      },
    };
  },
  computed: {
    filteredUsers() {
      const search = this.searchTerm.toLowerCase();
      return this.paginatedUsers.filter(
        (user) =>
          user.nombre.toLowerCase().includes(search) ||
          user.correo.toLowerCase().includes(search)
      );
    },
    paginatedUsers() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.users.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.users.length / this.itemsPerPage);
    },
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await api.get("/lista");
        this.users = response.data;
      } catch (error) {
        console.error("Error al obtener usuarios:", error);
      }
    },
    changePage(page) {
      if (page > 0 && page <= this.totalPages) {
        this.currentPage = page;
      }
    },
    openModal(action, user = null) {
      this.modalTitle = action === "crear" ? "Crear Usuario" : "Editar Usuario";
      this.modalAction = action === "crear" ? "Crear" : "Guardar Cambios";

      if (action === "editar" && user) {
        this.form = { ...user }; // Cargar datos del usuario seleccionado
      } else {
        this.resetForm(); // Limpiar formulario para crear
      }
      const modal = new Modal(document.getElementById("userModal"));
      modal.show();
    },
    resetForm() {
      this.form = { id: null, nombre: "", correo: "", edad: null };
    },
    async handleSubmit() {
    try {
      if (this.form.id) {
        // Editar usuario
        const { id, createdAt, updatedAt, ...userData } = this.form; 
        await api.put(`/${this.form.id}`, userData);
        this.users = this.users.map((user) =>
          user.id === this.form.id ? { ...this.form } : user
        );
      } else {
        // Crear usuario
        const { id, createdAt, updatedAt, ...userData } = this.form; 
        const response = await api.post("/", userData);
        this.users.push(response.data);
        this.fetchUsers();
      }
      const modal = Modal.getInstance(
        document.getElementById("userModal")
      );
      modal.hide(); 
      this.resetForm();
    } catch (error) {
      console.error("Error al guardar usuario:", error);
    }
  },
    async deleteUser(id) {
      try {
        await api.delete(`/${id}`);
        this.users = this.users.filter((user) => user.id !== id);
      } catch (error) {
        console.error("Error al eliminar usuario:", error);
      }
    },
  },
  mounted() {
    this.fetchUsers();
  },
};
</script>
<style>

/* Estilos para el modo oscuro */
.dark-mode .modal-content {
  background-color: #1e1e1e; 
  color: white;
}

.dark-mode .modal-header,
.dark-mode .modal-footer {
  background-color: #333;
}

.dark-mode .btn-primary {
  background-color: #007bff; 
  border-color: #007bff;
}

.dark-mode .btn-primary:hover {
  background-color: #0056b3;
  border-color: #004085;
}

.dark-mode .btn-close {
  color: white; 
}

/* Estilos para el modo claro */
.light-mode .modal-content {
  background-color: white; 
  color: black; 
}

.light-mode .modal-header,
.light-mode .modal-footer {
  background-color: #f8f9fa; 
}

.light-mode .btn-primary {
  background-color: #007bff;
  border-color: #007bff;
}

.light-mode .btn-primary:hover {
  background-color: #0056b3;
  border-color: #004085;
}

.light-mode .btn-close {
  color: black;
}


</style>