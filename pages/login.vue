<template>
  <div class="main">
    <app-header/>

    <form @submit.prevent="onSubmit()">
      <h3>Login</h3>
      
      <input class="input" type="email" placeholder="Email" v-model="email">
      <input class="input" type="password" placeholder="Password" v-model="password">

      <p class="error" v-for="error in errors" :key="error">• {{ error }}</p>

      <button class="btn blue" type="submit">Login</button>
    </form>
  </div>
</template>

<script>
import AppHeader from '~/components/AppHeader.vue';
import axios from 'axios';

export default {
  components: { AppHeader },
  head: {
    title: 'Login'
  },
  data() {
    return {
      email: '',
      password: '',
      errors: []
    }
  },
  methods: {
    async onSubmit() {
      const config = {
        headers: {
          Accept: 'application/json'
        }
      }
      const data = {
        identifier: this.email,
        password: this.password
      }

      try {
        const res = await axios.post('http://localhost:1337/api/auth/local', data);

        localStorage.setItem('jwt', res.data.jwt);
        localStorage.setItem('user', JSON.stringify(res.data.user));

        this.$router.push({ path: '/notes' })
      } catch (error) {
        this.errors.push('An error occured. Email or Password may be incorrect')
      }
      
    }
  }
}
</script>

<style>

</style>