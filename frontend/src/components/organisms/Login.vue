<template>
  <form @submit.prevent="login">
    <TextField
      v-model="email"
      required
      type="email"
      name="email"
      label="Email Address"
      placeholder="Enter your email address"
    />

    <TextField
      v-model="password"
      required
      minlength="6"
      type="password"
      maxlength="50"
      placeholder="Enter your full password"
      label="Password"
    />

    <input type="checkbox" id="remember" v-model="remember" />Remember me

    <div class="w-full pt-5">
      <Button type="submit" label="Submit" />
    </div>

    <p style="color: green" v-if="logged">Logged in successfully</p>
  </form>
</template>

<script setup>
import { useMutation } from '@vue/apollo-composable';
import { ref } from 'vue';
import { LOGIN_USER } from '../../graphql';
import TextField from '../atoms/TextField.vue';
import Button from '../atoms/Button.vue';

const email = ref('');
const password = ref('');
const remember = ref(false);
let logged = ref(false);

const { mutate: loginUser } = useMutation(LOGIN_USER, () => ({
  variables: {
    email: email,
    password: password,
    remember,
  },
}));

const login = () => {
  const user = loginUser();
  if (user) {
    // Save State and Redirect to Dashboard
    logged.value = true;
  }
};
</script>

<style scoped>
input[type='email'],
input[type='password'] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type='submit'] {
  width: 100%;
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type='submit']:hover {
  background-color: #45a049;
}

form {
  width: 50%;
  margin: 0 auto;
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
</style>
