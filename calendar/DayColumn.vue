<!-- DayColumn.vue -->
<template>
  <div class="px-3 py-3">
    <div class="font-medium text-slate-700">
      {{ day.toLocaleDateString('en-US', { weekday: 'short' }) }}
    </div>
    <div class="text-xs text-slate-500">
      {{ day.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }) }}
    </div>
  </div>
</template>

<script setup>
defineProps({
  day: {
    type: Date,
    required: true
  }
})
</script>