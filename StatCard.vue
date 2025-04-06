<!-- StatCard.vue -->
<template>
  <div :class="['bg-white rounded-xl p-6 text-center relative overflow-hidden border border-gray-200',
    hoverable ? 'hover:shadow-lg transition-shadow duration-300' : '']">

    <!-- Background Grid Pattern -->
    <svg class="absolute inset-0 w-full h-full opacity-[0.03] pointer-events-none" preserveAspectRatio="none">
      <pattern id="grid-pattern" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
        <path d="M 20 0 L 0 0 0 20" fill="none" :stroke="theme" stroke-width="0.5"/>
      </pattern>
      <rect width="100%" height="100%" fill="url(#grid-pattern)" />
    </svg>

    <!-- Top Left Wave Decoration -->
    <!-- Top Left Wave Decoration -->
    <svg class="absolute -top-2 left-0 w-full h-24 opacity-10" viewBox="0 0 400 100">
      <path
          :stroke="theme"
          fill="none"
          stroke-width="2"
          d="M 0 40 Q 100 40, 150 20 Q 200 0, 300 0 Q 350 0, 375 20 Q 390 40, 400 40"
      />
      <path
          :stroke="theme"
          fill="none"
          stroke-width="2"
          d="M 0 60 Q 100 60, 150 40 Q 200 20, 300 20 Q 350 20, 375 40 Q 390 60, 400 60"
      />
    </svg>


    <!-- Bottom Right Wave Decoration -->
    <svg data-v-b6ba9a66="" class="absolute -bottom-2 right-0 w-full h-24 opacity-10 transform rotate-180" viewBox="0 0 350 80"><path data-v-b6ba9a66="" stroke="hotpink" fill="none" stroke-width="3" d="M 0 40 Q 100 40, 150 20 Q 200 0, 300 0 Q 350 0, 375 20 Q 390 40, 400 40"></path><path data-v-b6ba9a66="" stroke="hotpink" fill="none" stroke-width="2" d="M 0 60 Q 100 80, 150 50 Q 200 20, 300 20 Q 360 20, 375 40 Q 400 60, 400 60" style="
    rotate: 7deg;
"></path></svg>

    <!-- Icon Badge -->
    <div class="absolute top-4 right-4 text-4xl opacity-10 achievement-badge">
      {{ icon }}
    </div>

    <!-- Main Content -->
    <div class="relative z-10">
      <h2 class="text-5xl font-bold mb-2 tracking-tight" :style="{ color: theme }">
        {{ stats.value }}
      </h2>
      <p class="text-sm font-medium text-gray-600 mb-4 uppercase tracking-wider">
        {{ stats.label }}
      </p>

      <!-- Dots Indicator -->
      <div v-if="dots" class="flex justify-center space-x-2 mb-4">
        <div
            v-for="n in dots"
            :key="n"
            :class="['w-2 h-2 rounded-full transition-all duration-300',
            n <= dots ? 'bg-current' : 'bg-gray-200']"
            :style="{ color: theme }"
        ></div>
      </div>

      <!-- Increase Badge -->
      <div
          v-if="increase"
          class="inline-flex items-center px-3 py-1 rounded-full text-sm"
          :style="{ backgroundColor: `${theme}20`, color: theme }"
      >
        <svg
            class="w-4 h-4 mr-1"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
        >
          <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"
          />
        </svg>
        <span class="font-medium">+{{ increase }}% increase</span>
      </div>
    </div>

    <!-- Bottom Decorative Line -->
    <div class="absolute bottom-0 left-0 w-full h-px bg-gradient-to-r from-transparent via-current to-transparent opacity-10"
         :style="{ color: theme }">
    </div>
  </div>
</template>

<script>
export default {
  name: 'StatCard',
  props: {
    stats: {
      type: Object,
      required: true,
      validator: (obj) => 'value' in obj && 'label' in obj
    },
    theme: {
      type: String,
      default: '#1a365d'
    },
    icon: {
      type: String,
      required: true
    },
    increase: {
      type: [Number, String],
      default: null
    },
    dots: {
      type: Number,
      default: 0
    },
    hoverable: {
      type: Boolean,
      default: true
    }
  }
}
</script>

<style scoped>
.stat-animation {
  animation: statFloat 4s ease-in-out infinite;
}

@keyframes statFloat {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-6px); }
}

/* Smooth transitions */
.transition-all {
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 300ms;
}

/* Remove default outline but keep it for accessibility */
:focus {
  outline: 2px solid transparent;
  outline-offset: 2px;
}
.achievement-badge {
  position: absolute;
  top: -4px;
  right: -20px;
  font-size: 3rem;
  transform: rotate(45deg);
  opacity: 0.33;
}
/* Ensure text remains sharp during animations */
* {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>