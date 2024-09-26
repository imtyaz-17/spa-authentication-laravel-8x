<script setup>
import { ref } from 'vue';
import HelloWorld from './components/HelloWorld.vue';
import axios from 'axios';

const form = ref({
  email: '',
  password: ''
});
const userData = ref(null);

axios.defaults.withCredentials = true;
axios.defaults.withXSRFToken = true;

async function loginHandler() {
  try {
    await axios.get("http://localhost:8000/sanctum/csrf-cookie");
    const response = await axios.post("http://localhost:8000/login", {
      email: form.value.email,
      password: form.value.password
    });
    // console.log(response);
    userData.value = await axios.get("http://localhost:8000/api/user");

  } catch (error) {
    console.error(error);
  }
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>
  <div class="login-container">
    <h1>Login</h1>
    {{ userData }}
    <form @submit.prevent="loginHandler">
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" v-model="form.email" required />
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" v-model="form.password" required />
      </div>
      <button type="submit">Login</button>
    </form>
  </div>
</template>

<style scoped>
.login-container {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1.5rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

h1 {
  text-align: center;
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  padding: 0.5rem;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: #fff;
  font-size: 1rem;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
