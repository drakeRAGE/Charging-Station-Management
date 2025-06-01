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
  }
});
</script>

<template>
  <div class="station-card">
    <div class="card-header">
      <h3>{{ station.name }}</h3>
      <div class="actions">
        <button @click="() => edit(station)" class="btn btn-edit">Edit</button>
        <button @click="() => deletes(station?._id)" class="btn btn-delete">Delete</button>
      </div>
    </div>
    <p>Location: {{ station.location }}</p>
    <p>Status: {{ station.status }}</p>
    <p>Power: {{ station.powerOutput }} kW</p>
    <p>Connector Type: {{ station.connectorType }}</p>
  </div>
</template>

<style scoped>
.station-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin: 8px;
  background: white;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.actions {
  display: flex;
  gap: 8px;
}

.btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.btn-edit {
  background-color: #42b983;
  color: white;
}

.btn-delete {
  background-color: #ff4757;
  color: white;
}

.btn:hover {
  opacity: 0.9;
}
</style>