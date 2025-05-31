<template>
  <div class="home">
    <nav class="navbar">
      <router-link to="/home">Home</router-link>
      <router-link to="/stations">Stations</router-link>
    </nav>
    
    <div class="hero">
      <h1>Welcome to Charging Stations</h1>
      <p>Find and manage electric vehicle charging stations</p>
      <router-link to="/stations" class="btn">View Stations</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.navbar {
  display: flex;
  gap: 20px;
  padding: 10px 0;
  margin-bottom: 30px;
  border-bottom: 1px solid #eee;
}

.hero {
  text-align: center;
  padding: 80px 20px;
  background: #f5f5f5;
  border-radius: 8px;
  margin-bottom: 40px;
}

.hero h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
}

.hero p {
  font-size: 1.2rem;
  margin-bottom: 30px;
}

.btn {
  display: inline-block;
  padding: 12px 24px;
  background: #42b983;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background 0.3s;
}

.btn:hover {
  background: #3aa876;
}
</style>