<template>
    <div class="user-list-container">
      <header class="header">
        <h2>Lista de Usuarios</h2>
        <button @click="logout" class="btn btn-logout">Cerrar sesión</button>
      </header>
      <ul class="user-list">
        <li v-for="user in users" :key="user.id" class="user-item">
          <div>
            <strong>{{ user.first_name }} {{ user.last_name }}</strong>
          </div>
          <div>{{ user.email }}</div>
          <div class="action-buttons">
            <button @click="editUser(user)" class="btn btn-warning">Editar</button>
            <button @click="deleteUser(user.id)" class="btn btn-danger">Eliminar</button>
          </div>
        </li>
      </ul>
  
      <!-- MODAL PARA EDITAR -->
      <div v-if="isEditing" class="modal-overlay">
        <div class="modal-content">
          <h3>Edit User</h3>
          <form @submit.prevent="updateUser">
            <div class="input-group">
              <label for="edit-first-name">First Name:</label>
              <input type="text" v-model="editedUser.first_name" id="edit-first-name" required />
            </div>
            <div class="input-group">
              <label for="edit-last-name">Last Name:</label>
              <input type="text" v-model="editedUser.last_name" id="edit-last-name" required />
            </div>
            <div class="input-group">
              <label for="edit-email">Email:</label>
              <input type="email" v-model="editedUser.email" id="edit-email" required />
            </div>
            <button type="submit" class="btn btn-primary">Modificar</button>
            <button @click="cancelEdit" class="btn btn-secondary">Cancelar</button>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        users: [],
        isEditing: false,
        editedUser: {},
      };
    },
    created() {
      this.fetchUsers();
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await axios.get('http://localhost:8000/api/users', {
            headers: { Authorization: `Bearer ${localStorage.getItem('auth_token')}` },
          });
          this.users = response.data;
        } catch (error) {
          console.error('Error fetching users:', error);
        }
      },
      async deleteUser(userId) {
        try {
          await axios.delete(`http://localhost:8000/api/users/${userId}`, {
            headers: { Authorization: `Bearer ${localStorage.getItem('auth_token')}` },
          });
          this.users = this.users.filter(user => user.id !== userId);
        } catch (error) {
          console.error('Error deleting user:', error);
        }
      },
      editUser(user) {
        this.isEditing = true;
        this.editedUser = { ...user };
      },
      async updateUser() {
        try {
          const response = await axios.put(`http://localhost:8000/api/users/${this.editedUser.id}`, this.editedUser, {
            headers: { Authorization: `Bearer ${localStorage.getItem('auth_token')}` },
          });
          const index = this.users.findIndex(user => user.id === this.editedUser.id);
          this.$set(this.users, index, response.data);
          this.cancelEdit();
        } catch (error) {
          console.error('Error updating user:', error);
        }
      },
      cancelEdit() {
        this.isEditing = false;
        this.editedUser = {};
      },
      logout() {
        localStorage.removeItem('auth_token');
        this.$router.push({ name: 'Login' });
      },
    },
  };
  </script>
  
  <style scoped>
  .user-list-container {
    width: 80%;
    margin: 50px auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    border: 1px solid #ddd;
  }
  
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  
  h2 {
    margin: 0;
  }
  
  .btn-logout {
    background-color: #e74c3c;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-logout:hover {
    background-color: #c0392b;
  }
  
  .user-list {
    list-style-type: none;
    padding: 0;
  }
  
  .user-item {
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  
  .user-item:last-child {
    border-bottom: none;
  }
  
  strong {
    display: block;
    font-size: 1.1em;
  }
  
  .action-buttons {
    margin-top: 10px;
  }
  
  button {
    padding: 5px 10px;
    margin: 0 5px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-primary {
    background-color: #007bff;
    color: white;
  }
  
  .btn-primary:hover {
    background-color: #0056b3;
  }
  
  .btn-warning {
    background-color: #ffc107;
    color: white;
  }
  
  .btn-warning:hover {
    background-color: #e0a800;
  }
  
  .btn-danger {
    background-color: #dc3545;
    color: white;
  }
  
  .btn-danger:hover {
    background-color: #c82333;
  }
  
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    width: 400px;
  }
  
  .input-group {
    margin-bottom: 15px;
  }
  
  input {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  </style>
  