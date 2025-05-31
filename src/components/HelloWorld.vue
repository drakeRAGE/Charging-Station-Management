<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

</script>

<template>
  <div class="h-screen bg-purple-300 flex items-center justify-center">
    <h1 class="text-3xl font-serif text-pink-700">{{ msg }}</h1>
  </div>
</template>

<style scoped>

</style>
