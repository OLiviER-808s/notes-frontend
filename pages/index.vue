<template>
  <div class="main">
    <app-header/>
    <p>A simple note taking app built with Nuxt.js and Strapi</p>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import AppHeader from '~/components/AppHeader.vue'

export default Vue.extend({
  components: { AppHeader },
  name: 'IndexPage',
  head: {
    title: 'Note Maker'
  }
})
</script>
