<!-- ProgressBar.vue -->
<template>
  <div class="relative h-2 bg-slate-100/30 rounded-lg overflow-hidden backdrop-blur-sm mt-4" style="background: #dedede;">
    <!-- Background shimmer effect -->
    <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/10 to-transparent animate-shimmer"></div>

    <!-- Progress bar -->
    <div
        class="h-full bg-gradient-to-r from-blue-500 to-blue-400 transition-all duration-300 ease-out rounded-full shadow-lg relative"
        :style="{ width: `${progress}%` }"
    >
      <!-- Glow effect -->
      <div class="absolute inset-0 bg-blue-400/30 blur-sm"></div>

      <!-- Moving particles effect -->
      <div class="absolute inset-0 overflow-hidden">
        <div class="absolute inset-0 bg-gradient-to-r from-white/0 via-white/20 to-white/0 -skew-x-12 animate-slide"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  progress: {
    type: Number,
    default: 0,
    validator: (value) => value >= 0 && value <= 100
  }
});
</script>

<style scoped>
@keyframes shimmer {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

@keyframes slide {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

.animate-shimmer {
  animation: shimmer 2s infinite;
}

.animate-slide {
  animation: slide 1.5s infinite;
}
</style>