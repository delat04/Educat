<!-- WeeklyStats.vue -->
<template>
  <div class="p-6">
    <div class="grid grid-cols-1 sm:grid-cols-4 gap-4 mb-6">
      <div
          v-for="stat in stats"
          :key="stat.label"
          class="relative group bg-gradient-to-br from-slate-50 to-white p-6 rounded-xl
               border border-slate-200 shadow-md hover:shadow-lg
               transition-all duration-300 hover:scale-105"
      >
        <!-- Top Decorative Line -->
        <div
            class="absolute top-0 left-0 right-0 h-1 rounded-t-lg bg-gradient-to-r from-blue-500/40
                 via-indigo-500/40 to-purple-500/40 transform origin-left transition-transform
                 duration-500 scale-x-0 group-hover:scale-x-100"
        ></div>

        <!-- Label -->
        <div class="flex items-center gap-2 mb-2">
          <div class="w-2 h-2 rounded-full bg-blue-500/60"></div>
          <div class="text-sm font-medium text-slate-600 uppercase tracking-wider">
            {{ stat.label }}
          </div>
        </div>

        <!-- Value -->
        <div class="flex items-baseline gap-2">
          <div class="text-3xl  font-semibold bg-gradient-to-r from-slate-800 to-slate-600
                      bg-clip-text text-transparent">
            {{ stat.value }}
          </div>
          <div class="text-sm text-slate-500 font-medium">
            {{ stat.unit }}
          </div>
        </div>

        <!-- Optional Trend Indicator -->
        <div
            v-if="stat.trend"
            class="absolute bottom-3 right-4 text-sm"
            :class="stat.trend > 0 ? 'text-emerald-600' : 'text-rose-600'"
        >
          {{ stat.trend > 0 ? '+' : '' }}{{ stat.trend }}%
        </div>

        <!-- Decorative Background Element -->
        <div class="absolute inset-0 bg-gradient-to-r from-blue-50/0 via-blue-50/30
                    to-blue-50/0 transform -skew-y-12 opacity-0 group-hover:opacity-100
                    transition-opacity duration-300 rounded-xl -z-10">
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  stats: {
    type: Array,
    required: true,
    // Expected format:
    // {
    //   label: String,
    //   value: String | Number,
    //   unit: String (optional),
    //   trend: Number (optional)
    // }
  }
})
</script>