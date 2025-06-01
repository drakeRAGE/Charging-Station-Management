<script setup>
import { reactive } from 'vue';
const emit = defineEmits(['submit']);
const charger = defineProps({
  initialData: {
    type: Object,
    default: () => ({
      name: '',
      location: '',
      status: 'Active',
      powerOutput: '',
      connectorType: ''
    })
  }
});

const formData = reactive({ ...charger.initialData });

const handleSubmit = () => {
  emit('submit', formData);
};
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <div class="form-group">
      <label>Name</label>
      <input v-model="formData.name" required />
    </div>
    <div class="form-group">
      <label>Location</label>
      <input v-model="formData.location" required />
    </div>
    <div class="form-group">
      <label>Status</label>
      <select v-model="formData.status">
        <option value="Active">Active</option>
        <option value="Inactive">Inactive</option>
      </select>
    </div>
    <div class="form-group">
      <label>Power (kW)</label>
      <input v-model="formData.powerOutput" type="number" required />
    </div>
    <div class="form-group">
      <label>Connector Type</label>
      <input v-model="formData.connectorType" step="0.01" required />
    </div>
    <button type="submit">Submit</button>
  </form>
</template>

<style scoped>
.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
}

input,
select {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 0.5rem 1rem;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>