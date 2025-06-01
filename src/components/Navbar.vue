<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
  <nav class="navbar">
    <RouterLink to="/" class="nav-link">Home</RouterLink>
    <RouterLink to="/login" class="nav-link">Login</RouterLink>
    <RouterLink to="/register" class="nav-link">Register</RouterLink>
    <RouterLink to="/profile" class="nav-link">Profile</RouterLink>
    <RouterLink to="/stations" class="nav-link">Stations</RouterLink>
  </nav>
</template>

<style scoped>
.navbar {
  display: flex;
  gap: 16px;
  padding: 16px;
  background: #42b983;
  margin-bottom: 16px;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-weight: 500;
}

.nav-link:hover {
  text-decoration: underline;
}
</style>