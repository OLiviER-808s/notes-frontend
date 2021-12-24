<template>
  <header>
    <h1>
      <nuxt-link to="/">Note Maker</nuxt-link>
    </h1>

    <div class="spacer"></div>

    <nuxt-link class="link" to="/signup">Signup</nuxt-link>
    <nuxt-link class="link" to="/login">Login</nuxt-link>
  </header>
</template>

<script>
export default {
  
}
</script>

<style scoped>
  header {
    display: flex;
    align-items: center;
    margin-bottom: 1.5em;
  }

  header h1 {
    margin: 0;
  }

  header a {
    color: black;
    text-decoration: none;
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