<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-body-tertiary fixed-top">
      <div class="container-fluid">
        <a class="navbar-brand">Prueba Tecnica</a>
        <button class="btn btn-outline" @click="toggleDarkMode">Cambiar Modo</button>
      </div>
    </nav>
    <div class="d-flex justify-content-between align-items-center mt-5 mb-5 top-bar">
      <h1 class="mt-5">Gestión de Usuarios</h1>
      
    </div>
    <div class="d-flex flex-column align-items-center mt-1">
      
      
      <UserList/>
    </div>
  </div>
</template>

<script>
import UserList from "./components/UserList.vue";


export default {

  components: {
    UserList
  
  },

  data() {
    return {
      isDarkMode: false,
      users: [],
      searchQuery: "",
      showForm: false,
      selectedUser: null,
    };
  },
  
  methods: {
    
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
      document.body.classList.toggle("dark-mode", this.isDarkMode);
      document.body.classList.toggle("light-mode", !this.isDarkMode);
    },
  },
  mounted() {
    const storedMode = localStorage.getItem("darkMode") === "true";
    this.isDarkMode = storedMode;
    document.body.classList.toggle("dark-mode", storedMode);
    document.body.classList.toggle("light-mode", !storedMode);
  },
};
</script>

<style>
.app-container {
  padding: 2rem;
  
}


</style>
