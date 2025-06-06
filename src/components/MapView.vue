<script setup>
import { onMounted, ref, watch } from 'vue';

const props = defineProps({
  station: {
    type: Object,
    required: true
  },
  onClose: {
    type: Function,
    required: true
  }
});

const mapContainer = ref(null);
const coordinates = ref({ lat: 28.7041, lng: 77.1025 }); // Default coordinates (Delhi)
const loading = ref(true);
const error = ref('');

const EVENT_MAP_API_KEY = '9145d9d11bfd4ed7a8440431d5a89bbf'; // OpenCage API key

// Function to fetch coordinates from location string
const fetchCoordinates = async (location) => {
  try {
    loading.value = true;
    const response = await fetch(`https://api.opencagedata.com/geocode/v1/json?q=${encodeURIComponent(location)}&key=${EVENT_MAP_API_KEY}`);
    const data = await response.json();
    
    if (data.results && data.results.length > 0) {
      const { lat, lng } = data.results[0].geometry;
      coordinates.value = { lat, lng };
      error.value = '';
    } else {
      error.value = 'Location not found';
    }
  } catch (err) {
    error.value = 'Error fetching location data';
    console.error('Error fetching coordinates:', err);
  } finally {
    loading.value = false;
  }
};

// Initialize map with Leaflet
const initializeMap = () => {
  if (!window.L) {
    // Load Leaflet if not already loaded
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css';
    document.head.appendChild(link);

    const script = document.createElement('script');
    script.src = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js';
    script.onload = () => createMap();
    document.head.appendChild(script);
  } else {
    createMap();
  }
};

const createMap = () => {
  if (mapContainer.value && window.L) {
    // Clear existing map
    mapContainer.value.innerHTML = '';
    
    const map = window.L.map(mapContainer.value).setView([coordinates.value.lat, coordinates.value.lng], 13);
    
    window.L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    // Custom marker icon for EV stations
    const evIcon = window.L.divIcon({
      html: `<div style="background: linear-gradient(135deg, #667eea, #764ba2); width: 30px; height: 30px; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; font-size: 16px; border: 3px solid white; box-shadow: 0 2px 8px rgba(0,0,0,0.3);">⚡</div>`,
      iconSize: [36, 36],
      iconAnchor: [18, 18]
    });

    window.L.marker([coordinates.value.lat, coordinates.value.lng], { icon: evIcon })
      .addTo(map)
      .bindPopup(`
        <div style="text-align: center; min-width: 200px;">
          <h4 style="margin: 0 0 10px 0; color: #333; font-size: 16px;">${props.station.name}</h4>
          <p style="margin: 5px 0; font-size: 14px;"><strong>Location:</strong> ${props.station.location}</p>
          <p style="margin: 5px 0; font-size: 14px;"><strong>Status:</strong> <span style="color: ${getStatusColor(props.station.status)}; font-weight: 600;">${props.station.status}</span></p>
          <p style="margin: 5px 0; font-size: 14px;"><strong>Power:</strong> ${props.station.powerOutput} kW</p>
          <p style="margin: 5px 0; font-size: 14px;"><strong>Connector:</strong> ${props.station.connectorType}</p>
        </div>
      `)
      .openPopup();
  }
};

const getStatusColor = (status) => {
  switch (status) {
    case 'Online':
    case 'Active':
      return '#28a745';
    case 'Offline':
    case 'Inactive':
      return '#dc3545';
    case 'Maintenance':
      return '#ffc107';
    default:
      return '#666';
  }
};

const chargingStationTags = [
  'EV Charging', 'Electric Vehicle', 'Fast Charging', 'Clean Energy',
  'Sustainable Transport', 'Public Charging', 'Green Technology', 'Smart Grid',
  'Zero Emission', 'Renewable Energy', 'Carbon Neutral', 'Eco Friendly'
];

// Watch for station changes
watch(() => props.station, (newStation) => {
  if (newStation && newStation.location) {
    fetchCoordinates(newStation.location);
  }
}, { immediate: true });

// Watch for coordinate changes to update map
watch(coordinates, () => {
  if (!loading.value) {
    initializeMap();
  }
}, { deep: true });

onMounted(() => {
  if (props.station && props.station.location) {
    fetchCoordinates(props.station.location);
  }
});
</script>
<template>
  <div class="map-overlay">
    <div class="map-modal">
      <div class="modal-header">
        <h2>Station Location</h2>
        <button @click="onClose" class="close-btn">&times;</button>
      </div>
      
      <div v-if="loading" class="loading">
        <div class="spinner"></div>
        <p>Loading map...</p>
      </div>
      
      <div v-else-if="error" class="error">
        <p>{{ error }}</p>
        <button @click="fetchCoordinates(station.location)" class="retry-btn">Retry</button>
      </div>
      
      <div v-else class="map-content">
        <div class="flex flex-col lg:flex-row gap-6">
          <div class="flex-1">
            <div class="map-container">
              <h3 class="location-title">{{ station.name }}</h3>
              <div ref="mapContainer" class="map-view"></div>
            </div>
          </div>
          
          <div class="station-info">
            <div class="info-section">
              <h3>Station Details</h3>
              <div class="detail-item">
                <strong>Name:</strong> {{ station.name }}
              </div>
              <div class="detail-item">
                <strong>Location:</strong> {{ station.location }}
              </div>
              <div class="detail-item">
                <strong>Status:</strong> 
                <span :class="getStatusClass(station.status)">{{ station.status }}</span>
              </div>
              <div class="detail-item">
                <strong>Power Output:</strong> {{ station.powerOutput }} kW
              </div>
              <div class="detail-item">
                <strong>Connector Type:</strong> {{ station.connectorType }}
              </div>
            </div>
            
            <div class="info-section">
              <h3>Coordinates</h3>
              <p class="coordinates">{{ coordinates.lat.toFixed(6) }}, {{ coordinates.lng.toFixed(6) }}</p>
            </div>
            
            <div class="info-section">
              <h3>Tags</h3>
              <div class="tags-container">
                <span v-for="(tag, index) in stationTags" :key="index" class="tag">
                  {{ tag }}
                </span>
              </div>
            </div>
          </div>
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
.map-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.map-modal {
  background: white;
  border-radius: 12px;
  width: 95%;
  max-width: 1200px;
  max-height: 90%;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-bottom: 1px solid #eee;
  background: #f8f9fa;
}

.modal-header h2 {
  margin: 0;
  color: #333;
  font-size: 24px;
}

.close-btn {
  background: none;
  border: none;
  font-size: 30px;
  cursor: pointer;
  color: #666;
  padding: 0;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.close-btn:hover {
  background: #f0f0f0;
  color: #333;
}

.loading, .error {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 60px 20px;
  text-align: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #42b983;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.retry-btn {
  background: #42b983;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  margin-top: 10px;
}

.map-content {
  padding: 20px;
  max-height: 70vh;
  overflow-y: auto;
}

.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}

.flex-1 {
  flex: 1;
}

.gap-6 {
  gap: 24px;
}

.map-container {
  width: 100%;
}

.location-title {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 15px;
  color: #333;
}

.map-view {
  width: 100%;
  height: 400px;
  border-radius: 8px;
  border: 1px solid #ddd;
}

.station-info {
  width: 100%;
  max-width: 350px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.info-section {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.info-section h3 {
  margin: 0 0 15px 0;
  color: #333;
  font-size: 18px;
  font-weight: 600;
}

.detail-item {
  margin: 10px 0;
  color: #666;
}

.detail-item strong {
  color: #333;
}

.coordinates {
  font-family: monospace;
  color: #666;
  margin: 0;
  font-size: 14px;
}

.tags-container {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  background: #e9ecef;
  color: #495057;
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
}

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

@media (max-width: 1024px) {
  .flex-col.lg\:flex-row {
    flex-direction: column;
  }
  
  .station-info {
    max-width: 100%;
  }
}

@media (max-width: 768px) {
  .map-modal {
    width: 98%;
    max-height: 95%;
  }
  
  .modal-header {
    padding: 15px;
  }
  
  .modal-header h2 {
    font-size: 20px;
  }
  
  .map-content {
    padding: 15px;
  }
  
  .map-view {
    height: 300px;
  }
}
</style>