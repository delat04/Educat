<!-- StatusIndicator.vue -->
<template>
  <div class="relative group" style="top: -9px; right: -9px;">
    <div class="absolute -top-2 -right-2 flex items-center justify-center">
      <div
          :class="[
          'relative w-4 h-4 rounded-full transition-all duration-300',
          'shadow-lg ring-2',
          statusColorClass,
          isProcessing ? 'animate-pulse scale-110' : ''
        ]"
      >
        <!-- Inner glow -->
        <div
            :class="[
            'absolute inset-0 rounded-full blur-sm opacity-50',
            statusColorClass
          ]"
        />

        <!-- Ripple effect when processing -->
        <div
            v-if="isProcessing"
            :class="[
            'absolute -inset-1 rounded-full',
            'animate-ping opacity-75',
            statusColorClass
          ]"
        />

        <!-- Shine effect -->
        <div class="absolute inset-0 rounded-full bg-gradient-to-br from-white/50 to-transparent" />
      </div>

      <!-- Status Tooltip -->
      <div class="absolute opacity-0 group-hover:opacity-100 transition-opacity duration-200 bg-gray-800 text-white text-xs px-2 py-1 rounded-md -top-8 whitespace-nowrap">
        {{ statusText }}
        <div class="absolute -bottom-1 left-1/2 transform -translate-x-1/2 w-2 h-2 bg-gray-800 rotate-45"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {computed} from "vue";

const props = defineProps({
  status: {
    type: String,
    default: 'idle'
  },
  isProcessing: {
    type: Boolean,
    default: false
  }
});

const statusColorClass = computed(() => {
  const colors = {
    success: 'bg-emerald-500 ring-emerald-400/50',
    warning: 'bg-amber-500 ring-amber-400/50',
    error: 'bg-rose-500 ring-rose-400/50',
    info: 'bg-sky-500 ring-sky-400/50',
    idle: 'bg-slate-400 ring-slate-300/50'
  };
  return colors[props.status] || colors.idle;
});

const statusText = computed(() => {
  const texts = {
    success: 'Completed Successfully',
    error: 'Error Occurred',
    warning: 'Warning',
    info: 'In Progress',
    idle: 'Idle'
  };
  return texts[props.status] || 'Unknown Status';
});
</script>