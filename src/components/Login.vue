<template>
  <div>
    <h1>Login</h1>
    <input type="text" v-model="username" placeholder="Username">
    <input type="password" v-model="password" placeholder="Password">
    <button @click="login">Login</button>

    <h2>Register</h2>
    <input type="text" v-model="registerUsername" placeholder="Username">
    <input type="password" v-model="registerPassword" placeholder="Password">
    <button @click="register">Register</button>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';

    const username = ref('');
    const password = ref('');
    const registerUsername = ref('');
    const registerPassword = ref('');

    const login = () => {
      // go to welcome page.
    };

    const register = () => {
      //go to welcome page.
    };

    return {
      username,
      password,
      registerUsername,
      registerPassword,
      login,
      register,
    };

</script>