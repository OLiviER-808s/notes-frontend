<template>
  <div class="main">
    <app-header/>

    <form @submit.prevent="onSubmit()">
      <h3>Create Account</h3>
      
      <input class="input" type="email" placeholder="Email" v-model="email">
      <input class="input" type="password" placeholder="Password" v-model="password">
      <input class="input" type="password" placeholder="Confirm Password" v-model="confirmPassword">

      <p class="error" v-for="error in errors" :key="error">• {{ error }}</p>

      <button class="btn blue" type="submit">Create</button>
    </form>
  </div>
</template>

<script>
import AppHeader from '~/components/AppHeader.vue';
import axios from 'axios';

export default {
  components: { AppHeader },
  head: {
    title: 'Signup'
  },
  data () {
    return {
      email: '',
      password: '',
      confirmPassword: '',
      errors: []
    }
  },
  methods: {
    async onSubmit() {
      this.errors = []

      if (this.password.length < 6) {
        this.errors.push('Password must be 6 or more charcters');
      }
      if (this.password !== this.confirmPassword) {
        this.errors.push("Passwords don't match");
      }

      if (this.errors.length === 0) {
        const config = {
          headers: {
            Accept: 'application/json'
          }
        }
        const data = {
          email: this.email,
          password: this.password
        }

        try {
          const res = await axios.post('http://localhost:1337/api/auth/local/register', data, config);
          
          localStorage.setItem('jwt', res.data.jwt);
          localStorage.setItem('user', JSON.stringify(res.data.user));
          
          this.$router.push({ path: '/notes' })
        } catch (error) {
          this.errors.push('User with this email already exists');
        }
      }
    }
  }
}
</script>

<style>

</style>