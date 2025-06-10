<script setup lang="ts">
defineProps(['req'])
</script>

<template>
    <div class="md:col-span-1">
        <h2 class="text-lg font-semibold truncate">{{ req.name }}</h2>
    </div>
    <div class="md:col-span-2">
        <!-- Progress Bar -->
        <div class="w-full bg-gray-600 rounded-full h-6 flex overflow-hidden progress-bar">
            <div class="bg-green-500 h-full" :style="{ width: req.complete + '%' }"></div>
            <div class="bg-yellow-500 h-full" :style="{ width: req.remaining + '%' }"></div>
            <!--<div v-if="req.blocked" class="bg-red-500 h-full" :style="{ width: req.blocked + '%' }"></div>-->
            <div class="bg-red-500 h-full" :style="{ width: req.blocked + '%' }"></div>
        </div>
        <!-- Status Text -->
        <div class="flex justify-start text-sm mt-2 space-x-4">
            <span class="font-medium text-green-400">{{ req.complete }}% Complete</span>
            <span class="font-medium text-yellow-400">{{ req.remaining }}% Remaining</span>
            <span v-if="req.blocked" class="font-medium text-red-400">{{ req.blocked }}% Blocked</span>
        </div>
    </div>
</template>