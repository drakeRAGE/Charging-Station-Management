<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
import ChargerForm from '../components/ChargerForm.vue';
import ChargerCard from '../components/ChargerCard.vue';
import StationMap from '../components/MapView.vue';

const router = useRouter();
const stations = ref([]);
const isLoading = ref(false);
const error = ref(null);
const isEditing = ref(false);
const currentStationId = ref(null);
const selectedStation = ref(null);
const showMap = ref(false);

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

const handleCardClick = (station) => {
  selectedStation.value = station;
  showMap.value = true;
};

const closeMap = () => {
  showMap.value = false;
  selectedStation.value = null;
};

onMounted(() => {
  checkAuth();
  fetchStations();
});
</script>

<template>
  <div class="charging-station-container">
    <!-- Header Section -->
    <div class="header-section bg-inherit">
      <div class="header-content">
        <h1 class="page-title">
          <svg class="title-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <path d="M7 7h.01M7 3h5c.512 0 1.024.195 1.414.586l7 7a2 2 0 010 2.828l-7 7a2 2 0 01-2.828 0l-7-7A1.994 1.994 0 013 12V7a4 4 0 014-4z"/>
          </svg>
          Charging Station Management
        </h1>
        <p class="page-subtitle">Manage and monitor your EV charging network</p>
      </div>
    </div>

    <!-- Main Content Grid -->
    <div class="main-grid">
      <!-- Map Section -->
      <div class="map-section">
        <div class="section-header">
          <h2 class="section-title">
            <svg class="section-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z"/>
              <circle cx="12" cy="10" r="3"/>
            </svg>
            {{ showMap && selectedStation ? selectedStation.name + ' Location' : 'Network Overview' }}
          </h2>
          <button 
            v-if="showMap" 
            @click="closeMap"
            class="close-map-btn"
          >
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <line x1="18" y1="6" x2="6" y2="18"/>
              <line x1="6" y1="6" x2="18" y2="18"/>
            </svg>
            Close Map
          </button>
        </div>
        
        <div class="map-container">
          <!-- Show map when station is selected -->
          <div v-if="showMap && selectedStation" class="inline-map-wrapper">
            <div class="inline-map-content">
              <div class="station-details">
                <h3>{{ selectedStation.name }}</h3>
                <p><strong>Location:</strong> {{ selectedStation.location }}</p>
                <p><strong>Status:</strong> 
                  <span :class="getStatusClass(selectedStation.status)">{{ selectedStation.status }}</span>
                </p>
                <p><strong>Power:</strong> {{ selectedStation.powerOutput }} kW</p>
                <p><strong>Connector:</strong> {{ selectedStation.connectorType }}</p>
              </div>
              <StationMap
                :station="selectedStation"
                :inline="true"
              />
            </div>
          </div>
          
          <!-- Default placeholder when no station is selected -->
          <div v-else class="map-placeholder">
            <svg class="map-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path d="M3 12h18m-9-9v18"/>
              <circle cx="12" cy="12" r="3"/>
            </svg>
            <h3>Interactive Map</h3>
            <p>Click on any station card to view its location</p>
            <div class="map-instructions">
              <div class="instruction-item">
                <span class="instruction-icon">👆</span>
                <span>Click on a station card below</span>
              </div>
              <div class="instruction-item">
                <span class="instruction-icon">🗺️</span>
                <span>View detailed location map</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Form Section -->
      <div class="form-section">
        <div class="section-header">
          <h2 class="section-title">
            <svg class="section-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <circle cx="12" cy="12" r="10"/>
              <line x1="12" y1="8" x2="12" y2="16"/>
              <line x1="8" y1="12" x2="16" y2="12"/>
            </svg>
            {{ isEditing ? 'Edit Station' : 'Add New Station' }}
          </h2>
          <button 
            v-if="isEditing" 
            @click="isEditing = false; currentStationId = null"
            class="cancel-btn"
          >
            Cancel
          </button>
        </div>
        <div class="form-container">
          <ChargerForm 
            v-if="isEditing" 
            :initial-data="stations.find(s => s._id === currentStationId)"
            @submit="handleSubmit" 
            @cancel="isEditing = false; currentStationId = null" 
          />
          <ChargerForm v-else @submit="handleSubmit" />
        </div>
      </div>
    </div>

    <!-- Stations List Section -->
    <div class="stations-section">
      <div class="section-header">
        <h2 class="section-title">
          <svg class="section-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <path d="M12 2L2 7v10c0 5.55 3.84 10 9 11 1.16-.21 2.31-.48 3.38-.86"/>
            <path d="M22 12c0 .83-.15 1.64-.42 2.4M18 12l-2-2v8"/>
          </svg>
          Available Stations
          <span class="station-count">{{ stations.length }}</span>
        </h2>
      </div>
      
      <div class="stations-content">
        <!-- Loading State -->
        <div v-if="isLoading" class="loading-state">
          <div class="loading-spinner"></div>
          <p>Loading stations...</p>
        </div>
        
        <!-- Error State -->
        <div v-else-if="error" class="error-state">
          <svg class="error-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <circle cx="12" cy="12" r="10"/>
            <line x1="15" y1="9" x2="9" y2="15"/>
            <line x1="9" y1="9" x2="15" y2="15"/>
          </svg>
          <h3>Error Loading Stations</h3>
          <p>{{ error }}</p>
          <button @click="fetchStations" class="retry-btn">Try Again</button>
        </div>
        
        <!-- Empty State -->
        <div v-else-if="stations.length === 0" class="empty-state">
          <svg class="empty-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <circle cx="12" cy="12" r="10"/>
            <path d="M8 12h8M12 8v8"/>
          </svg>
          <h3>No Stations Found</h3>
          <p>Get started by adding your first charging station</p>
        </div>
        
        <!-- Stations Grid -->
        <div v-else class="stations-grid">
          <ChargerCard 
            v-for="station in stations" 
            :key="station._id" 
            :station="station" 
            :edit="handleEdit"
            :deletes="handleDelete"
            :on-card-click="handleCardClick"
            :class="{ 'selected-card': selectedStation && selectedStation._id === station._id }"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    getStatusClass(status) {
      return {
        'status-online': status === 'Online' || status === 'Active',
        'status-offline': status === 'Offline' || status === 'Inactive',
        'status-maintenance': status === 'Maintenance'
      };
    }
  }
};
</script>

<style scoped>
/* Global Styles */
* {
  box-sizing: border-box;
}

.charging-station-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 0;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}

/* Header Section */
.header-section {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  padding: 2rem 0;
  margin-bottom: 2rem;
}

.header-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.page-title {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-size: 2.5rem;
  font-weight: 700;
  color: #1a202c;
  margin: 0 0 0.5rem 0;
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.title-icon {
  width: 2.5rem;
  height: 2.5rem;
  stroke: #667eea;
  stroke-width: 2;
}

.page-subtitle {
  font-size: 1.125rem;
  color: #64748b;
  margin: 0;
}

/* Main Grid Layout */
.main-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto 2rem auto;
  padding: 0 2rem;
}

/* Section Styles */
.map-section,
.form-section,
.stations-section {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.2);
  overflow: hidden;
  transition: all 0.3s ease;
}

.map-section:hover,
.form-section:hover,
.stations-section:hover {
  transform: translateY(-2px);
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.15);
}

.section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 2rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.section-title {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-size: 1.5rem;
  font-weight: 600;
  color: #1a202c;
  margin: 0;
}

.section-icon {
  width: 1.5rem;
  height: 1.5rem;
  stroke: #667eea;
  stroke-width: 2;
}

.station-count {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.875rem;
  font-weight: 600;
  margin-left: 0.5rem;
}

/* Close Map Button */
.close-map-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: #ef4444;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 0.875rem;
}

.close-map-btn svg {
  width: 1rem;
  height: 1rem;
  stroke-width: 2;
}

.close-map-btn:hover {
  background: #dc2626;
  transform: translateY(-1px);
}

/* Map Container */
.map-container {
  padding: 2rem;
}

/* Inline Map Wrapper */
.inline-map-wrapper {
  height: 400px;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.inline-map-content {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.station-details {
  background: #f8f9fa;
  padding: 1rem;
  border-bottom: 1px solid #dee2e6;
}

.station-details h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.125rem;
  font-weight: 600;
  color: #333;
}

.station-details p {
  margin: 0.25rem 0;
  font-size: 0.875rem;
  color: #666;
}

.station-details strong {
  color: #333;
}

/* Map Placeholder */
.map-placeholder {
  height: 400px;
  border: 2px dashed #cbd5e0;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  transition: all 0.3s ease;
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.02), rgba(118, 75, 162, 0.02));
}

.map-placeholder:hover {
  border-color: #667eea;
  background: linear-gradient(135deg, rgba(102, 126, 234, 0.05), rgba(118, 75, 162, 0.05));
}

.map-icon {
  width: 3rem;
  height: 3rem;
  stroke: #64748b;
  stroke-width: 1.5;
}

.map-placeholder h3 {
  font-size: 1.25rem;
  font-weight: 600;
  color: #374151;
  margin: 0;
}

.map-placeholder p {
  color: #64748b;
  margin: 0;
  text-align: center;
}

.map-instructions {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-top: 1rem;
}

.instruction-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: #64748b;
}

.instruction-icon {
  font-size: 1rem;
}

/* Form Container */
.form-container {
  padding: 2rem;
}

.cancel-btn {
  background: #ef4444;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.cancel-btn:hover {
  background: #dc2626;
  transform: translateY(-1px);
}

/* Stations Section */
.stations-section {
  grid-column: 1 / -1;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.stations-content {
  padding: 2rem;
}

/* State Styles */
.loading-state,
.error-state,
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 3rem;
  text-align: center;
  gap: 1rem;
}

.loading-spinner {
  width: 3rem;
  height: 3rem;
  border: 3px solid #e2e8f0;
  border-top: 3px solid #667eea;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error-icon,
.empty-icon {
  width: 4rem;
  height: 4rem;
  stroke: #64748b;
  stroke-width: 1.5;
}

.error-state h3,
.empty-state h3 {
  font-size: 1.5rem;
  font-weight: 600;
  color: #374151;
  margin: 0;
}

.error-state p,
.empty-state p {
  color: #64748b;
  margin: 0;
}

.retry-btn {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.retry-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 10px 25px -5px rgba(102, 126, 234, 0.3);
}

/* Stations Grid */
.stations-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 1.5rem;
}

/* Selected Card Highlight */
.selected-card {
  border-color: #667eea !important;
  box-shadow: 0 8px 25px rgba(102, 126, 234, 0.2) !important;
  transform: translateY(-1px) !important;
}

.selected-card::before {
  content: '';
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-radius: 14px;
  z-index: -1;
}

/* Status Classes */
.status-online {
  color: #28a745;
  font-weight: 600;
}

.status-offline {
  color: #dc3545;
  font-weight: 600;
}

.status-maintenance {
  color: #ffc107;
  font-weight: 600;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .main-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
    padding: 0 1rem;
  }
  
  .header-content {
    padding: 0 1rem;
  }
  
  .page-title {
    font-size: 2rem;
  }
  
  .stations-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1rem;
  }
}

@media (max-width: 640px) {
  .header-content {
    text-align: center;
  }
  
  .page-title {
    font-size: 1.75rem;
    flex-direction: column;
    gap: 0.5rem;
  }
  
  .section-header {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .section-title {
    font-size: 1.25rem;
  }
  
  .stations-grid {
    grid-template-columns: 1fr;
  }
  
  .map-placeholder,
  .inline-map-wrapper {
    height: 300px;
  }
  
  .map-instructions {
    display: none;
  }

  .inline-map-content {
    flex-direction: column;
  }
  
  .station-details {
    padding: 0.75rem;
  }
}
</style>