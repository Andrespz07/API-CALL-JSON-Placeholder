<script setup>
import { ref } from 'vue';

const users = ref([]);
const userId = ref(null);
const user = ref(null);

const getUser = async () => {
  if (!userId.value) {
    alert('Por favor ingrese un ID de usuario');
    return;
  }
  const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId.value}`);
  user.value = await response.json();
}

const mounted = async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/users');
  users.value = await response.json();
}

mounted();
</script>

<template>
  <div>
    <h1>Lista de usuarios</h1>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Ciudad</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.address.city }}</td>
        </tr>
      </tbody>
    </table>
    <form @submit.prevent="getUser">
      <label for="userId">Buscar usuario por ID:</label>
      <input type="number" id="userId" v-model="userId" required>
      <button type="submit">Buscar</button>
    </form>
    <div v-if="user">
      <h2>{{ user.name }}</h2>
      <p>Teléfono: {{ user.phone }}</p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
 h1 {
  font-size: 2em;
  margin-bottom: 1em;

}

table {
  width: 100%;
  border-collapse: collapse;

  th,
  td {
    padding: 0.5em;
    border: 1px solid #ccc;
  }

  th {
    background-color: #eee;
  }
}

form {
  margin-top: 1em;

  label {
    display: inline-block;
    margin-right: 0.5em;
  }

  input[type="number"] {
    width: 4em;
    padding: 0.5em;
  }

  button[type="submit"] {
    padding: 0.5em 1em;
    background-color: #eee;
    border: none;
    cursor: pointer;

    &:hover {
      background-color: #ddd;
    }
  }
}

div[v-if] {
  margin-top: 1em;

  h2 {
    font-size: 1.5em;
  }

  p {
    margin-bottom: 0.5em;
  }
}

</style>
