<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
import ChargerForm from '../components/ChargerForm.vue';
import ChargerCard from '../components/ChargerCard.vue';

const router = useRouter();
const stations = ref([]);
const isLoading = ref(false);
const error = ref(null);
const isEditing = ref(false);
const currentStationId = ref(null);

const apiUrl = import.meta.env.VITE_API_URL;

const checkAuth = () => {
  const token = localStorage.getItem('token');
  if (!token) {
    router.push('/login');
    return;
  }
  return token;
};

const fetchStations = async () => {
  try {
    isLoading.value = true;

    const { data } = await axios.get(`${apiUrl}/api/charging-stations`);
    console.log("fetching data", data)
    stations.value = data;
  } catch (err) {
    error.value = err.response?.data?.message || err.message;
  } finally {
    isLoading.value = false;
  }
};

const handleSubmit = async (formData) => {
  const token = checkAuth();
  try {
    if (isEditing.value) {

      await axios.put(`${apiUrl}/api/charging-stations/${currentStationId.value}`, formData, {
        headers: { Authorization: `Bearer ${token}` }
      });
    } else {
      console.log("formData", formData)
      await axios.post(`${apiUrl}/api/charging-stations`, formData, {
        headers: { Authorization: `Bearer ${token}` }
      });
    }
    await fetchStations();
    isEditing.value = false;
    currentStationId.value = null;
  } catch (err) {
    error.value = err.response?.data?.message || err.message;
  }
};

const handleEdit = (station) => {
  isEditing.value = true;
  currentStationId.value = station._id;
};

const handleDelete = async (id) => {
  const token = checkAuth();
  try {

    await axios.delete(`${apiUrl}/api/charging-stations/${id}`, {
      headers: { Authorization: `Bearer ${token}` }
    });
    await fetchStations();
  } catch (err) {
    error.value = err.response?.data?.message || err.message;
  }
};

onMounted(() => {
  checkAuth();
  fetchStations();
});




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
      <h3>{{ isEditing ? 'Edit' : 'Add' }} Charging Station</h3>
      <ChargerForm v-if="isEditing" :initial-data="stations.find(s => s._id === currentStationId)"
        @submit="handleSubmit" @cancel="isEditing = false; currentStationId = null" />
      <ChargerForm v-else @submit="handleSubmit" />
    </div>

    <div class="station-list">
      <h3>Available Stations</h3>
      <div v-if="isLoading">Loading...</div>
      <div v-else-if="error">{{ error }}</div>
      <div v-else>
        <ChargerCard v-for="station in stations" :key="station._id" :station="station" :edit="handleEdit"
          :deletes="handleDelete" />
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

.map-container,
.station-form,
.station-list {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
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