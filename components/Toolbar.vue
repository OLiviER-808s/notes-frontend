<template>
  <header>
    <h1>Note Maker</h1>

    <div class="spacer"></div>

    <p>Logged in as <strong>{{ email }}</strong></p>
    <a @click="logout()" class="link">Logout</a>
  </header>
</template>

<script>
export default {
  data() {
    return {
      email: ''
    }
  },
  async created() {
    this.email = JSON.parse(localStorage.getItem('user')).email;
  },
  methods: {
    logout() {
      localStorage.clear();
      this.$router.push({ path: '/' });
    }
  }
}
</script>

<style scoped>
  header {
    display: flex;
    align-items: center;
    margin-bottom: 1.5em;
    width: 100%;
  }

  header h1 {
    margin: 0;
  }

  header a {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }

  .link {
    margin-left: 1em;
    padding: 0.5em;
  }

  .link:hover {
    background-color: rgb(221, 219, 219);
    border-radius: 7px;
  }
</style>