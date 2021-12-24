<template>
  <div class="container">
    <nuxt/>
  </div>
</template>

<script>
export default {
  
}
</script>

<style>
  body {
    font-family: Arial, Helvetica, sans-serif;
    background: #f1f1f1;
  }

  .container {
    display: flex;
    justify-content: center;
    padding: 1em;
  }

  .main {
    border: solid black 2px;
    padding: 2em;
    max-width: 600px;
  }

  .spacer {
    flex: 1 1 auto;
  }

  .input {
    border-radius: 7px;
    height: 1.5em;
    background-color: rgb(221, 219, 219);
    color: black;
    border: none;
    display: block;
    margin-bottom: 1.5em;
    padding: 0.5em;
    width: 95%;
    max-width: 250px;
  }

  .error {
    color: red;
  }

  .btn {
    padding: 0.5em 1em;
    font-size: 0.92rem;
    border: none;
    cursor: pointer;
    border-radius: 7px;
  }
  .blue {
    background-color: rgb(4, 195, 253);
  }
  .green {
    background-color: rgb(37, 209, 94);
  }
  .red {
    background-color: rgb(223, 16, 16);
  }
</style>