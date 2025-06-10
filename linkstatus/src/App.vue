<script setup lang="ts">
import RequirementStatus from './components/RequirementStatus.vue'
import { onMounted } from 'vue'

import { ref } from 'vue';

const requirements = ref([]);
const loading = ref(true);
const error = ref(null);
const apiUrl = 'https://raw.githubusercontent.com/inpursuit/linkstatus/refs/heads/main/status.json';

const fetchRequirements = async () => {
  loading.value = true;
  error.value = null;
  try {
    /*
    const mockData = [
      { name: "Basic Host", complete: 80, remaining: 20, blocked: 0 },
      { name: "J2.x PPLI", complete: 70, remaining: 20, blocked: 10 },
      { name: "J12.0 Mission Assignment", complete: 20, remaining: 60, blocked: 20 },
      { name: "Advanced Sensor Integration", complete: 45, remaining: 45, blocked: 10 },
      { name: "UI Overhaul", complete: 95, remaining: 5, blocked: 0 }
    ];
    await new Promise(res => setTimeout(res, 1000));
    requirements.value = mockData;
    */
    const response = await fetch(apiUrl);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    requirements.value = await response.json();
  } catch (e) {
    if (e instanceof Error) {
      error.value = e.message;
    }
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchRequirements();
});
</script>

<template>
  <h1 class="text-2xl sm:text-3xl font-bold mb-6 text-center">System Requirements Status</h1>
  
  <!-- Loading spinner -->
  <div v-if="loading" class="flex justify-center items-center h-64">
        <div class="animate-spin rounded-full h-16 w-16 border-t-4 border-b-4 border-blue-500"></div>
  </div>
  
  <!-- Error message -->
  <div v-if="error" class="bg-red-900 text-red-200 p-4 rounded-lg text-center">
      <p><strong>Error:</strong> {{ error }}</p>
      <p>Could not fetch status data. Please check the URL or your network connection.</p>
  </div>
  
  <!-- Status rows -->
  <div v-if="!loading && !error" class="space-y-6">
      <div v-for="req in requirements" :key="req.name" class="bg-gray-700 p-4 rounded-lg shadow-lg grid grid-cols-1 md:grid-cols-3 gap-4 items-center">
          <RequirementStatus :req="req" />
      </div>
  </div>
</template>

<style scoped>
</style>
