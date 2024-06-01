<template>
  <div id="app">
    <div v-if="isAuthenticated">
      <button @click="logout" class="btn btn-secondary">Logout</button>
      <Student />
    </div>
    <Login v-else @login-success="handleLoginSuccess" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Login from './components/Login.vue';
import Student from './components/Student.vue';

const isAuthenticated = ref(false);

const handleLoginSuccess = () => {
  isAuthenticated.value = true;
};

const logout = () => {
  isAuthenticated.value = false;
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>