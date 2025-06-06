<script setup>
const props = defineProps({
  station: {
    type: Object,
    required: true
  },
  edit: {
    type: Function,
    required: true
  },
  deletes: {
    type: Function,
    required: true
  },
  onCardClick: {
    type: Function,
    required: false
  }
});

const handleCardClick = () => {
  if (props.onCardClick) {
    props.onCardClick(props.station);
  }
};
</script>

<template>
  <div class="station-card" @click="handleCardClick">
    <div class="card-header">
      <h3>{{ station.name }}</h3>
      <div class="actions" @click.stop>
        <button @click="() => edit(station)" class="btn btn-edit">Edit</button>
        <button @click="() => deletes(station?._id)" class="btn btn-delete">Delete</button>
      </div>
    </div>
    <p><strong>Location:</strong> {{ station.location }}</p>
    <p><strong>Status:</strong> 
      <span :class="getStatusClass(station.status)">{{ station.status }}</span>
    </p>
    <p><strong>Power:</strong> {{ station.powerOutput }} kW</p>
    <p><strong>Connector Type:</strong> {{ station.connectorType }}</p>
    <div class="card-footer">
      <small class="text-muted">🗺️ Click to view location on map</small>
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
.station-card {
  border: 1px solid #ddd;
  border-radius: 12px;
  padding: 20px;
  margin: 12px;
  background: white;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.station-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
  border-color: #42b983;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  padding-bottom: 10px;
  border-bottom: 1px solid #eee;
}

.card-header h3 {
  margin: 0;
  color: #333;
  font-size: 18px;
  font-weight: 600;
}

.actions {
  display: flex;
  gap: 8px;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.2s ease;
}

.btn-edit {
  background-color: #42b983;
  color: white;
}

.btn-edit:hover {
  background-color: #369870;
}

.btn-delete {
  background-color: #ff4757;
  color: white;
}

.btn-delete:hover {
  background-color: #ff3838;
}

.station-card p {
  margin: 8px 0;
  color: #666;
  font-size: 14px;
}

.station-card p strong {
  color: #333;
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

.card-footer {
  margin-top: 15px;
  padding-top: 10px;
  border-top: 1px solid #eee;
  text-align: center;
}

.text-muted {
  color: #999;
  font-size: 12px;
}
</style>