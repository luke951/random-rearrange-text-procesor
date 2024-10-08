<template>
  <div class="container">
    <h1 class="title text-center">Home page</h1>
    <word-processor></word-processor>
  </div>
</template>

<script setup>
import { defineAsyncComponent } from 'vue'
const WordProcessor = defineAsyncComponent(() => import('./components/WordProcessor/WordProcessor.vue'))
</script>
