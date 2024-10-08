<template>
  <div class="container">
    <h1 class="title text-center">Home page</h1>
    <task-form></task-form>
  </div>
</template>

<script setup>
import { defineAsyncComponent } from 'vue'
const TaskForm = defineAsyncComponent(() => import('./components/WordProcessor/WordProcessor.vue'))
</script>
