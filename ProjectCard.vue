<template>
  <div class="relative max-w-md overflow-hidden bg-gray-800 rounded-lg shadow-2xl p-6 text-gray-100 group hover:shadow-indigo-500/20" style="margin: 10px;
    font-family: poppins,sans-serif;">
    <!-- Animated background gradient -->
    <div class="absolute inset-0 bg-gradient-to-br from-indigo-900/20 via-purple-900/20 to-pink-900/20 opacity-50 group-hover:opacity-100 transition-opacity duration-500"></div>

    <!-- Floating particles effect -->
    <div class="absolute inset-0 overflow-hidden">
      <div v-for="n in 5" :key="n"
           class="particle absolute w-1 h-1 rounded-full bg-indigo-400/30"
           :style="getParticleStyle(n)">
      </div>
    </div>

    <!-- Main content -->
    <div class="relative">
      <!-- Header section -->
      <div class="flex items-center justify-between mb-6">
        <div class="flex items-center space-x-4">
          <div class="relative">
            <!-- Animated flame container -->
            <div :class="[
              'p-3 rounded-xl transform transition-all duration-300',
              'bg-gradient-to-br from-gray-700 to-gray-800',
              'group-hover:scale-110 group-hover:rotate-3'
            ]">
              <div :class="[
                'flame-wrapper relative',
                {'animate-burn': streak > 0}
              ]">
                <Flame
                    :class="[
                    'w-8 h-8 transform transition-all duration-300',
                    getFlameColor
                  ]"
                />
                <!-- Flame particles -->
                <div v-if="streak > 0" class="absolute -top-1 left-1/2 w-px h-2">
                  <div v-for="n in 3" :key="n"
                       class="absolute w-1 h-1 rounded-full bg-orange-400/30 animate-float"
                       :style="`animation-delay: ${n * 200}ms`">
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="transform transition-all duration-300 group-hover:translate-x-2">
            <h3 class="text-xl font-bold bg-gradient-to-r from-white to-gray-300 bg-clip-text text-transparent">
              {{ projectTitle }}
            </h3>
            <p class="text-sm text-gray-400">
              {{ streak }} days streak
              <span class="ml-2 text-xs" v-if="streak > 0">🔥</span>
            </p>
          </div>
        </div>

        <div class="flex items-center space-x-3 bg-gray-700/50 px-3 py-1.5 rounded-full">
          <Clock class="w-4 h-4 text-gray-400" />
          <span class="text-sm font-medium">{{ formatTime(timeSpent) }}</span>
        </div>
      </div>

      <!-- Progress section -->
      <div class="mb-6 transform transition-all duration-300 group-hover:translate-y-1">
        <div class="flex justify-between mb-2">
          <span class="text-sm text-gray-400">Progress</span>
          <span class="text-sm font-medium">{{ progress }}%</span>
        </div>
        <div class="relative w-full h-2 bg-gray-700 rounded-full overflow-hidden">
          <div class="absolute inset-0 bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 rounded-full transition-all duration-700 ease-out"
               :style="{ transform: `translateX(${progress - 100}%)` }">
          </div>
          <!-- Shine effect -->
          <div class="absolute inset-0 w-full h-full bg-gradient-to-r from-transparent via-white/10 to-transparent animate-shine"></div>
        </div>
      </div>

      <!-- Subtasks section -->
      <div class="space-y-2">
        <button @click="isExpanded = !isExpanded"
                class="flex items-center space-x-2 text-gray-400 hover:text-gray-200 transition-colors">
          <component :is="isExpanded ? 'ChevronDown' : 'ChevronRight'"
                     class="w-4 h-4 transition-transform duration-300"
                     :class="{ 'rotate-90': !isExpanded }" />
          <span class="text-sm">Subtasks ({{ subtasks.length }})</span>
        </button>

        <TransitionGroup
            name="list"
            tag="div"
            class="pl-4 border-l border-gray-700 space-y-2"
        >
          <div v-if="isExpanded"
               v-for="task in subtasks"
               :key="task.id"
               class="flex items-center justify-between py-2 px-3 rounded-lg bg-gray-700/30 hover:bg-gray-700/50 transition-colors">
            <span class="text-sm">{{ task.name }}</span>
            <span class="text-xs text-gray-400">{{ formatTime(task.time) }}</span>
          </div>
        </TransitionGroup>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { Flame, Clock, ChevronDown, ChevronRight } from 'lucide-vue-next'

// Props
const props = defineProps({
  projectTitle: {
    type: String,
    default: 'Project Alpha'
  },
  streak: {
    type: Number,
    default: 7
  },
  timeSpent: {
    type: Number, // in minutes
    default: 150
  },
  progress: {
    type: Number,
    default: 75
  },
  subtasks: {
    type: Array,
    default: () => ([
      { id: 1, name: 'Research & Planning', time: 30 },
      { id: 2, name: 'Design Implementation', time: 60 },
      { id: 3, name: 'Testing & Review', time: 60 }
    ])
  }
})

// State
const isExpanded = ref(false)

// Computed
const getFlameColor = computed(() => {
  if (props.streak === 0) return 'text-gray-400'
  if (props.streak < 3) return 'text-orange-400'
  if (props.streak < 7) return 'text-orange-500'
  return 'text-orange-600'
})

// Methods
const formatTime = (minutes) => {
  const hours = Math.floor(minutes / 60)
  const mins = minutes % 60
  return `${hours}h ${mins}m`
}

const getParticleStyle = (index) => {
  const randomDelay = Math.random() * 3
  return {
    left: `${Math.random() * 100}%`,
    animationDelay: `${randomDelay + index}s`
  }
}
</script>

<style scoped>
.particle {
  animation: float 3s infinite linear;
}

.animate-burn {
  animation: burn 1s infinite alternate;
}

.animate-shine {
  animation: shine 3s infinite linear;
}

@keyframes float {
  0% {
    transform: translateY(100%) translateX(0);
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    transform: translateY(-100%) translateX(20px);
    opacity: 0;
  }
}

@keyframes burn {
  0% {
    transform: scale(1) rotate(0deg);
  }
  100% {
    transform: scale(1.1) rotate(3deg);
  }
}

@keyframes shine {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(100%);
  }
}

.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}
</style>