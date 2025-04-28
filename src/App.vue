<script setup lang="ts">
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
</script>

<template>
  <Navbar />
  <main class="pt-16"> <!-- Add padding-top to account for fixed navbar -->
    <RouterView />
  </main>
</template>
