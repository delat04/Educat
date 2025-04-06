<!-- CalendarHeader.vue -->
<template>
  <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-6">
    <div>
      <h2 class="text-xl font-bold text-slate-800">
        Academic Schedule
      </h2>
      <p class="text-sm text-slate-600">
        Week of {{ weekStart.toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' }) }}
      </p>
    </div>
    <div class="flex gap-2">
      <button
          v-for="size in ['compact', 'normal', 'expanded']"
          :key="size"
          @click="$emit('update:viewSize', size)"
          class="px-3 py-1 text-sm rounded-md"
          :class="viewSize === size ? 'bg-indigo-100 text-indigo-700' : 'bg-slate-100 hover:bg-slate-200 text-slate-700'"
      >
        {{ size.charAt(0).toUpperCase() + size.slice(1) }}
      </button>
      <div class="border-l border-slate-200 mx-2"></div>
      <button class="px-3 py-1 bg-slate-100 hover:bg-slate-200 rounded-md" @click="$emit('previous-week')">
        &larr;
      </button>
      <button class="px-3 py-1 bg-slate-100 hover:bg-slate-200 rounded-md" @click="$emit('next-week')">
        &rarr;
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  weekStart: {
    type: Date,
    required: true
  },
  viewSize: {
    type: String,
    required: true
  }
})

defineEmits(['update:viewSize', 'previous-week', 'next-week'])
</script>