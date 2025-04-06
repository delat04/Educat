<template>
  <div class="smart-device-card" :class="{ 'device-on': isOn }">
    <!-- Main Card Container -->
    <div class="w-72 bg-gradient-to-br from-slate-800 to-slate-900 rounded-xl p-6 shadow-xl hover:shadow-2xl transform hover:scale-105 transition-all duration-300 relative overflow-hidden">
      <!-- Animated Glow Background -->
      <div :class="`absolute inset-0 bg-gradient-to-r from-blue-500/10 to-purple-500/10 transition-opacity duration-500 ${isOn ? 'opacity-100' : 'opacity-0'}`">
        <div class="absolute inset-0 animate-pulse bg-gradient-to-t from-transparent to-white/5"></div>
      </div>

      <!-- Smart Light Icon -->
      <div class="relative mb-4">
        <svg
            viewBox="0 0 24 24"
            :class="`w-12 h-12 ${isOn ? 'text-yellow-400' : 'text-gray-400'} transition-colors duration-300`"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
        >
          <path d="M12 2C8.13 2 5 5.13 5 9c0 2.38 1.19 4.47 3 5.74V17c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-2.26c1.81-1.27 3-3.36 3-5.74 0-3.87-3.13-7-7-7z" />
          <path d="M9 21h6" />
          <path d="M12 3v1M3.5 8.5l1 1M20.5 8.5l-1 1" />
        </svg>
      </div>

      <!-- Device Info -->
      <div class="space-y-4">
        <!-- Header -->
        <div class="flex justify-between items-center">
          <h3 class="text-lg font-semibold text-white">Smart Light</h3>
          <!-- Power Button -->
          <button
              @click="toggleDevice"
              :class="`w-12 h-6 rounded-full relative transition-colors duration-300 ${isOn ? 'bg-green-500' : 'bg-gray-600'}`"
          >
            <span
                :class="`absolute w-5 h-5 rounded-full bg-white transform transition-transform duration-300 ${isOn ? 'translate-x-6' : 'translate-x-1'}`"
            ></span>
          </button>
        </div>

        <!-- Status Indicators -->
        <div class="grid grid-cols-2 gap-2 text-sm">
          <div class="bg-slate-700/50 rounded-lg p-2">
            <p class="text-gray-400">Brightness</p>
            <p class="text-white font-medium">{{ brightness }}%</p>
          </div>
          <div class="bg-slate-700/50 rounded-lg p-2">
            <p class="text-gray-400">Power</p>
            <p class="text-white font-medium">{{ power }}W</p>
          </div>
        </div>

        <!-- Brightness Slider -->
        <div class="space-y-2">
          <input
              type="range"
              v-model="brightness"
              class="w-full h-2 rounded-lg appearance-none cursor-pointer bg-slate-700"
              min="0"
              max="100"
          />
        </div>

        <!-- Color Temperature -->
        <div class="flex space-x-2">
          <div
              v-for="colorOption in colorOptions"
              :key="colorOption"
              @click="selectedColor = colorOption"
              :class="`w-6 h-6 rounded-full cursor-pointer transform transition hover:scale-110 ${selectedColor === colorOption ? 'ring-2 ring-white' : ''}`"
              :style="{ backgroundColor: colorOption }"
          ></div>
        </div>

        <!-- Additional Info -->
        <div class="text-xs text-gray-400 flex items-center space-x-2">
          <span class="flex items-center">
            <div class="w-2 h-2 rounded-full bg-green-500 mr-1"></div>
            Connected
          </span>
          <span>|</span>
          <span>Room: Living Room</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SmartDeviceCard',
  data() {
    return {
      isOn: false,
      brightness: 80,
      power: 8.5,
      selectedColor: '#FFD700',
      colorOptions: ['#FFD700', '#FFA07A', '#87CEEB', '#98FB98', '#DDA0DD']
    }
  },
  methods: {
    toggleDevice() {
      this.isOn = !this.isOn
      // Simulate power consumption change
      this.power = this.isOn ? 8.5 : 0
    }
  },
  watch: {
    brightness(newVal) {
      if (this.isOn) {
        this.power = (newVal / 100 * 8.5).toFixed(1)
      }
    }
  }
}
</script>

<style scoped>
.smart-device-card input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  background: transparent;
}

.smart-device-card input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 16px;
  height: 16px;
  background: #ffffff;
  border-radius: 50%;
  cursor: pointer;
  margin-top: -6px;
  transition: all 0.2s ease;
}

.smart-device-card input[type="range"]::-webkit-slider-runnable-track {
  height: 4px;
  background: #4B5563;
  border-radius: 2px;
}

.smart-device-card input[type="range"]::-webkit-slider-thumb:hover {
  transform: scale(1.1);
}

@keyframes glow {
  0% { opacity: 0.5; }
  50% { opacity: 0.8; }
  100% { opacity: 0.5; }
}

.device-on .glow {
  animation: glow 2s infinite;
}
</style>