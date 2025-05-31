<script setup>
const emit = defineEmits(['filter']);

const filters = reactive({
  status: '',
  minPower: null,
  maxPower: null,
  maxPrice: null,
  location: ''
});

const handleFilter = () => {
  emit('filter', filters);
};

const resetFilters = () => {
  Object.assign(filters, {
    status: '',
    minPower: null,
    maxPower: null,
    maxPrice: null,
    location: ''
  });
  emit('filter', filters);
};
</script>

<template>
  <div class="filter-controls">
    <div class="filter-group">
      <label>Status</label>
      <select v-model="filters.status">
        <option value="">All</option>
        <option value="available">Available</option>
        <option value="in-use">In Use</option>
        <option value="maintenance">Maintenance</option>
      </select>
    </div>
    
    <div class="filter-group">
      <label>Min Power (kW)</label>
      <input v-model="filters.minPower" type="number" min="0" />
    </div>
    
    <div class="filter-group">
      <label>Max Power (kW)</label>
      <input v-model="filters.maxPower" type="number" min="0" />
    </div>
    
    <div class="filter-group">
      <label>Max Price ($/kWh)</label>
      <input v-model="filters.maxPrice" type="number" min="0" step="0.01" />
    </div>
    
    <div class="filter-group">
      <label>Location</label>
      <input v-model="filters.location" type="text" />
    </div>
    
    <div class="filter-actions">
      <button @click="handleFilter">Apply Filters</button>
      <button @click="resetFilters">Reset</button>
    </div>
  </div>
</template>

<style scoped>
.filter-controls {
  padding: 16px;
  background: #f5f5f5;
  border-radius: 8px;
  margin-bottom: 16px;
}

.filter-group {
  margin-bottom: 12px;
}

label {
  display: block;
  margin-bottom: 4px;
  font-weight: 500;
}

select, input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.filter-actions {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:first-child {
  background: #42b983;
  color: white;
}

button:last-child {
  background: #f0f0f0;
}
</style>