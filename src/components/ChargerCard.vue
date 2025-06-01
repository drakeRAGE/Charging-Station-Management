<script setup>
const props = defineProps({
  station: {
    type: Object,
    required: true
  }
});
</script>

<template>
  <div class="station-card">
    <h3>{{ station.name }}</h3>
    <p>Location: {{ station.location }}</p>
    <p>Status: {{ station.status }}</p>
    <p>Power: {{ station.powerOutput }} kW</p>
    <p>connector Type: ${{ station.connectorType }}/kWh</p>
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
</style>