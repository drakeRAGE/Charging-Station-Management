<script setup>
const props = defineProps({
  charger: {
    type: Object,
    required: true
  }
});
</script>

<template>
  <div class="charger-card">
    <h3>{{ charger.name }}</h3>
    <p>Location: {{ charger.location }}</p>
    <p>Status: {{ charger.status }}</p>
    <p>Power: {{ charger.power }} kW</p>
    <p>Price: ${{ charger.price }}/kWh</p>
  </div>
</template>

<style scoped>
.charger-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin: 8px;
  background: white;
}
</style>