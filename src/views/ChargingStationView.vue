<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const checkAuth = () => {
  const token = localStorage.getItem('token');
  if (!token) {
    router.push('/login');
    return;
  }
};

onMounted(() => {
  checkAuth();
});

const stations = ref([
  { id: 1, name: 'Station 1', location: 'Downtown', available: true },
  { id: 2, name: 'Station 2', location: 'Uptown', available: false },
  { id: 3, name: 'Station 3', location: 'Midtown', available: true }
]);

const newStation = ref({
  name: '',
  location: '',
  available: false
});

const addStation = () => {
  stations.value.push({
    id: stations.value.length + 1,
    ...newStation.value
  });
  newStation.value = { name: '', location: '', available: false };
};




</script>

<template>
  <div class="charging-station">
    <div class="map-container">
      <!-- Map will be implemented here -->
      <h3>Map View</h3>
      <div class="map-placeholder">
        Map will be displayed here
      </div>
    </div>
    
    <div class="station-form">
      <h3>Add Charging Station</h3>
      <form @submit.prevent="addStation">
        <div class="form-group">
          <label>Name</label>
          <input v-model="newStation.name" type="text" required />
        </div>
        <div class="form-group">
          <label>Location</label>
          <input v-model="newStation.location" type="text" required />
        </div>
        <div class="form-group">
          <label>
            <input v-model="newStation.available" type="checkbox" />
            Available
          </label>
        </div>
        <button type="submit" class="btn">Add Station</button>
      </form>
    </div>
    
    <div class="station-list">
      <h3>Available Stations</h3>
      <div v-for="station in stations" :key="station.id" class="station-card">
        <h4>{{ station.name }}</h4>
        <p>Location: {{ station.location }}</p>
        <p>Status: {{ station.available ? 'Available' : 'Occupied' }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.charging-station {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  padding: 20px;
}

.map-container, .station-form, .station-list {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.map-placeholder {
  height: 300px;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 10px;
}

.station-form {
  grid-column: 2;
}

.station-list {
  grid-column: 1 / -1;
}

.station-card {
  padding: 15px;
  margin: 10px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.btn {
  background: #42b983;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn:hover {
  background: #3aa876;
}
</style>