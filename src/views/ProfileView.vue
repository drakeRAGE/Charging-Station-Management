<template>
  <div class="profile">
    <h2>Profile</h2>
    <div v-if="user" class="profile-info">
      <p><strong>Name:</strong> {{ user.name }}</p>
      <p><strong>Email:</strong> {{ user.email }}</p>
      <p><strong>IsAdmin:</strong>{{ user.isAdmin }}</p>
      <button @click="handleLogout" class="btn">Logout</button>
    </div>
    <div v-else>
      <p>Loading user data...</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProfileView',
  data() {
    return {
      user: null
    }
  },
  async created() {
    try {
      const token = localStorage.getItem('token');
      if (!token) {
        this.$router.push('/login');
        return;
      }

      const response = await fetch('https://charging-station-backend-odxc.onrender.com/api/auth/me', {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      });

      if (response.ok) {
        this.user = await response.json();
      } else {
        localStorage.removeItem('token');
        this.$router.push('/login');
      }
    } catch (error) {
      console.error('Profile error:', error);
      localStorage.removeItem('token');
      this.$router.push('/login');
    }
  },
  methods: {
    handleLogout() {
      localStorage.removeItem('token');
      this.$router.push('/login');
    }
  }
}
</script>

<style scoped>
.profile {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.profile-info {
  margin-bottom: 20px;
}

button {
  background-color: #42b983;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #369f6b;
}
</style>