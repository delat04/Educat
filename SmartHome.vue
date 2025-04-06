<!-- SmartHome.vue -->
<template>
  <div class="relative min-h-screen bg-gradient-to-br from-slate-900 to-slate-800 overflow-hidden">
    <!-- Floating particles background -->
    <div class="absolute inset-0 overflow-hidden">
      <div v-for="particle in particles"
           :key="particle.id"
           :style="getParticleStyle(particle)"
           class="absolute bg-blue-400/20 rounded-full pointer-events-none">
      </div>
    </div>

    <!-- Main content -->
    <div class="relative z-10 container mx-auto px-4 py-8">
      <!-- Hero Text Animation -->
      <div class="text-center mb-16">
        <h1 class="text-4xl md:text-6xl font-bold text-white mb-4">
          Imagine Your
          <span class="relative inline-block">
            <transition-group name="slide" tag="span" class="absolute left-0">
              <span v-for="(text, index) in rotatingTexts"
                    :key="text"
                    v-show="currentTextIndex === index"
                    class="block bg-gradient-to-r from-cyan-500 to-blue-500 bg-clip-text text-transparent">
                {{ text }}
              </span>
            </transition-group>
          </span>
        </h1>
      </div>

      <!-- Smart Features Grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
        <div v-for="feature in smartFeatures"
             :key="feature.id"
             @mouseenter="activeFeature = feature.id"
             @mouseleave="activeFeature = null"
             class="relative group">
          <!-- Feature Card -->
          <div class="relative bg-slate-800/50 backdrop-blur-lg rounded-xl p-6 border border-slate-700
                      transition-all duration-300 hover:scale-105 hover:bg-slate-700/50">
            <!-- Pulse Animation -->
            <div class="absolute inset-0 -z-10">
              <div v-for="i in 4" :key="i"
                   :class="[
                     'absolute inset-0 rounded-xl border border-slate-600/50',
                     `animate-pulse-${i}`
                   ]">
              </div>
            </div>

            <!-- Icon -->
            <div class="w-16 h-16 mb-4 rounded-full bg-gradient-to-br from-cyan-500 to-blue-500
                        flex items-center justify-center">
              <component :is="feature.icon"
                         class="w-8 h-8 text-white"/>
            </div>

            <!-- Content -->
            <h3 class="text-xl font-bold text-white mb-2">{{ feature.title }}</h3>
            <p class="text-slate-300">{{ feature.description }}</p>

            <!-- Status Indicator -->
            <div class="absolute top-4 right-4 flex items-center space-x-2">
              <div class="w-2 h-2 rounded-full"
                   :class="feature.active ? 'bg-green-500' : 'bg-slate-500'">
              </div>
              <span class="text-sm text-slate-400">
                {{ feature.active ? 'Active' : 'Standby' }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue';
import {
  Sun,
  Wind,
  Thermometer,
  Shield,
  BatteryCharging,
  Home,
  Lock,
  Video
} from 'lucide-vue-next';

export default {
  name: 'SmartHome',
  components: {
    Sun,
    Wind,
    Thermometer,
    Shield,
    BatteryCharging,
    Home,
    Lock,
    Video
  },
  setup() {
    const activeFeature = ref(null);
    const currentTextIndex = ref(0);
    const particles = ref([]);

    const rotatingTexts = [
      'Smart Home',
      'Energy Efficient',
      'Safe Space',
      'Future Living'
    ];

    const smartFeatures = [
      {
        id: 'lighting',
        icon: 'Sun',
        title: 'Smart Lighting',
        description: 'Adaptive lighting that responds to natural light and your daily routine',
        active: true
      },
      {
        id: 'climate',
        icon: 'Thermometer',
        title: 'Climate Control',
        description: 'AI-powered temperature and humidity management',
        active: true
      },
      {
        id: 'security',
        icon: 'Shield',
        title: 'Security',
        description: 'Advanced monitoring with facial recognition and anomaly detection',
        active: true
      },
      {
        id: 'energy',
        icon: 'BatteryCharging',
        title: 'Energy Management',
        description: 'Smart power distribution and consumption optimization',
        active: false
      },
      {
        id: 'ventilation',
        icon: 'Wind',
        title: 'Air Quality',
        description: 'Real-time air quality monitoring and automatic ventilation',
        active: true
      },
      {
        id: 'surveillance',
        icon: 'Video',
        title: 'Surveillance',
        description: '24/7 smart monitoring with AI detection',
        active: true
      },
      {
        id: 'access',
        icon: 'Lock',
        title: 'Access Control',
        description: 'Biometric and smart device-based access management',
        active: false
      },
      {
        id: 'automation',
        icon: 'Home',
        title: 'Home Automation',
        description: 'Custom scenes and automated routines',
        active: true
      }
    ];

    // Text rotation
    const rotateText = () => {
      currentTextIndex.value = (currentTextIndex.value + 1) % rotatingTexts.length;
    };
    const textInterval = setInterval(rotateText, 3000);

    // Generate particles
    onMounted(() => {
      particles.value = Array.from({ length: 50 }, (_, i) => ({
        id: i,
        x: Math.random() * 100,
        y: Math.random() * 100,
        size: Math.random() * 3 + 1,
        speed: Math.random() + 0.5,
      }));
    });

    // Cleanup
    onUnmounted(() => {
      clearInterval(textInterval);
    });

    const getParticleStyle = (particle) => {
      return {
        left: `${particle.x}%`,
        top: `${particle.y}%`,
        width: `${particle.size}px`,
        height: `${particle.size}px`,
        animation: `float ${particle.speed}s infinite linear`
      };
    };

    return {
      activeFeature,
      currentTextIndex,
      rotatingTexts,
      smartFeatures,
      particles,
      getParticleStyle
    };
  }
};
</script>

<style scoped>
@keyframes float {
  0% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(10px, 10px);
  }
  100% {
    transform: translate(0, 0);
  }
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.5s ease;
}

.slide-enter-from {
  opacity: 0;
  transform: translateY(20px);
}

.slide-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

@keyframes pulse-1 {
  0% { transform: scale(1); opacity: 0.8; }
  100% { transform: scale(1.5); opacity: 0; }
}

@keyframes pulse-2 {
  0% { transform: scale(1); opacity: 0.6; }
  100% { transform: scale(1.7); opacity: 0; }
}

@keyframes pulse-3 {
  0% { transform: scale(1); opacity: 0.4; }
  100% { transform: scale(1.9); opacity: 0; }
}

@keyframes pulse-4 {
  0% { transform: scale(1); opacity: 0.2; }
  100% { transform: scale(2.1); opacity: 0; }
}

.animate-pulse-1 { animation: pulse-1 3s infinite; }
.animate-pulse-2 { animation: pulse-2 3s infinite 0.75s; }
.animate-pulse-3 { animation: pulse-3 3s infinite 1.5s; }
.animate-pulse-4 { animation: pulse-4 3s infinite 2.25s; }
</style>