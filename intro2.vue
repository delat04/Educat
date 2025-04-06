<template>
  <div class="max-w-4xl mx-auto p-6 space-y-8">
    <!-- Interactive Header -->
    <div class="relative overflow-hidden bg-gradient-to-r from-blue-600 to-indigo-600 rounded-xl p-8 text-white">
      <div class="relative z-10">
        <h1 class="text-3xl font-bold mb-4">
          Multiple Depot Vehicle Routing Problem (MDVRP)
        </h1>
        <p class="text-lg opacity-90">
          {{ currentDescription }}
        </p>
      </div>
      <!-- Animated background elements -->
      <div v-for="(dot, index) in 5" :key="index"
           class="absolute w-24 h-24 bg-white opacity-10 rounded-full"
           :style="randomPosition(index)">
      </div>
    </div>

    <!-- Interactive Problem Visualization -->
    <div class="bg-white rounded-xl shadow-lg p-6">
      <h2 class="text-xl font-semibold mb-6">Understanding the Problem</h2>

      <div class="grid md:grid-cols-2 gap-8">
        <!-- Interactive Diagram -->
        <div class="relative h-64 bg-gray-50 rounded-lg p-4">
          <svg width="100%" height="100%" @mousemove="updateHoverEffect">
            <!-- Depots -->
            <circle v-for="(depot, index) in depots"
                    :key="'depot-'+index"
                    :cx="depot.x"
                    :cy="depot.y"
                    r="15"
                    class="fill-blue-500 transition-all duration-300"
                    :class="{'scale-110': activeElement === `depot-${index}`}"
                    @mouseenter="setActiveElement(`depot-${index}`)"
                    @mouseleave="setActiveElement(null)"/>

            <!-- Customers -->
            <circle v-for="(customer, index) in customers"
                    :key="'customer-'+index"
                    :cx="customer.x"
                    :cy="customer.y"
                    r="8"
                    class="fill-green-500 transition-all duration-300"
                    :class="{'scale-110': activeElement === `customer-${index}`}"
                    @mouseenter="setActiveElement(`customer-${index}`)"
                    @mouseleave="setActiveElement(null)"/>

            <!-- Routes -->
            <path v-for="(route, index) in routes"
                  :key="'route-'+index"
                  :d="generatePath(route)"
                  class="stroke-gray-400 stroke-2 fill-none"
                  :class="{'animate-dash': activeElement === `route-${index}`}"
                  @mouseenter="setActiveElement(`route-${index}`)"
                  @mouseleave="setActiveElement(null)"/>
          </svg>
        </div>

        <!-- Interactive Legend -->
        <div class="space-y-4">
          <div v-for="(item, index) in elements"
               :key="index"
               class="p-3 rounded-lg transition-all duration-300"
               :class="[
                 item.bgClass,
                 activeElement === item.id ? 'scale-105 shadow-md' : ''
               ]"
               @mouseenter="setActiveElement(item.id)"
               @mouseleave="setActiveElement(null)">
            <h3 class="font-semibold flex items-center gap-2">
              <div :class="item.iconClass">
                {{ item.icon }}
              </div>
              {{ item.title }}
            </h3>
            <p class="text-sm text-gray-600 mt-1">{{ item.description }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- GA Steps with Animation -->
    <div class="bg-white rounded-xl shadow-lg p-6">
      <h2 class="text-xl font-semibold mb-6">Genetic Algorithm Solution</h2>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <!-- Step Timeline -->
        <div class="space-y-4">
          <div v-for="(step, index) in gaSteps"
               :key="index"
               class="relative pl-8 pb-8"
               :class="{'opacity-50': currentStep !== null && currentStep !== index}">
            <!-- Timeline line -->
            <div v-if="index < gaSteps.length - 1"
                 class="absolute left-4 top-8 w-0.5 h-full bg-gray-200"></div>

            <!-- Step circle -->
            <div class="absolute left-0 top-0 w-8 h-8 rounded-full bg-blue-500 text-white flex items-center justify-center">
              {{ index + 1 }}
            </div>

            <!-- Step content -->
            <div class="ml-4">
              <h3 class="font-semibold text-lg">{{ step.title }}</h3>
              <p class="text-gray-600 mt-1">{{ step.description }}</p>
              <button
                  @click="currentStep = index"
                  class="mt-2 px-3 py-1 text-sm bg-blue-50 text-blue-600 rounded-full hover:bg-blue-100 transition-colors">
                Learn more
              </button>
            </div>
          </div>
        </div>

        <!-- Step Details -->
        <div class="bg-gray-50 rounded-xl p-6">
          <template v-if="currentStep !== null">
            <h3 class="font-semibold text-lg mb-4">{{ gaSteps[currentStep].title }}</h3>
            <div class="space-y-4">
              <div v-for="(detail, index) in gaSteps[currentStep].details"
                   :key="index"
                   class="flex items-start gap-3">
                <div class="w-6 h-6 rounded-full bg-blue-100 text-blue-600 flex-shrink-0 flex items-center justify-center">
                  {{ index + 1 }}
                </div>
                <p class="text-gray-600">{{ detail }}</p>
              </div>
            </div>
          </template>
          <div v-else class="text-center text-gray-500">
            Select a step to see details
          </div>
        </div>
      </div>
    </div>

    <!-- Key Metrics -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div v-for="(metric, index) in metrics"
           :key="index"
           class="bg-white rounded-xl shadow-lg p-6">
        <div class="text-sm text-gray-500 mb-1">{{ metric.label }}</div>
        <div class="text-2xl font-semibold">{{ metric.value }}</div>
        <div class="mt-2 text-sm" :class="metric.trend.color">
          {{ metric.trend.value }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'MDVRPIntroduction',

  setup() {
    const descriptions = [
      "A complex optimization problem in logistics and supply chain management.",
      "Efficiently routing vehicles from multiple depots to serve customer demands.",
      "Minimizing total distance while respecting vehicle and depot capacities."
    ];

    const currentDescription = ref(descriptions[0]);
    const descriptionIndex = ref(0);

    const activeElement = ref(null);
    const currentStep = ref(null);

    // Simulated data for visualization
    const depots = [
      { x: 50, y: 50 },
      { x: 200, y: 50 }
    ];

    const customers = [
      { x: 80, y: 120 },
      { x: 150, y: 150 },
      { x: 180, y: 80 }
    ];

    const routes = [
      { from: { x: 50, y: 50 }, to: { x: 80, y: 120 } },
      { from: { x: 200, y: 50 }, to: { x: 180, y: 80 } }
    ];

    const elements = [
      {
        id: 'depot',
        icon: '🏪',
        iconClass: 'text-blue-500',
        title: 'Depots',
        description: 'Starting points with limited capacity and vehicle fleets',
        bgClass: 'bg-blue-50'
      },
      {
        id: 'customer',
        icon: '👤',
        iconClass: 'text-green-500',
        title: 'Customers',
        description: 'Locations with specific demands that need to be served',
        bgClass: 'bg-green-50'
      },
      {
        id: 'route',
        icon: '🚚',
        iconClass: 'text-purple-500',
        title: 'Routes',
        description: 'Optimized paths connecting depots to customers',
        bgClass: 'bg-purple-50'
      }
    ];

    const gaSteps = [
      {
        title: 'Population Initialization',
        description: 'Create initial set of random solutions',
        details: [
          'Generate random customer-to-depot assignments',
          'Ensure capacity constraints are met',
          'Calculate initial fitness scores'
        ]
      },
      {
        title: 'Selection',
        description: 'Choose the fittest solutions for reproduction',
        details: [
          'Evaluate solution quality based on total distance',
          'Consider depot capacity utilization',
          'Select top performing solutions as parents'
        ]
      },
      {
        title: 'Crossover',
        description: 'Combine good solutions to create better ones',
        details: [
          'Exchange customer assignments between parent solutions',
          'Maintain feasibility of new solutions',
          'Create diversity in solution population'
        ]
      },
      {
        title: 'Mutation',
        description: 'Introduce small random changes',
        details: [
          'Randomly reassign customers to different depots',
          'Modify route sequences',
          'Help avoid local optima'
        ]
      }
    ];

    const metrics = [
      {
        label: 'Average Route Length',
        value: '42.3 km',
        trend: { value: '↓ 12% from initial', color: 'text-green-500' }
      },
      {
        label: 'Depot Utilization',
        value: '87%',
        trend: { value: '↑ 15% efficiency', color: 'text-green-500' }
      },
      {
        label: 'Vehicles Required',
        value: '8',
        trend: { value: '↓ 2 from baseline', color: 'text-green-500' }
      }
    ];

    // Methods
    const setActiveElement = (id) => {
      activeElement.value = id;
    };

    const generatePath = (route) => {
      return `M ${route.from.x} ${route.from.y} L ${route.to.x} ${route.to.y}`;
    };

    const randomPosition = (index) => {
      const positions = [
        { top: '10%', left: '10%' },
        { top: '60%', left: '20%' },
        { top: '30%', right: '10%' },
        { bottom: '20%', right: '20%' },
        { bottom: '10%', left: '40%' }
      ];
      return positions[index];
    };

    // Cycle through descriptions
    onMounted(() => {
      setInterval(() => {
        descriptionIndex.value = (descriptionIndex.value + 1) % descriptions.length;
        currentDescription.value = descriptions[descriptionIndex.value];
      }, 5000);
    });

    return {
      currentDescription,
      activeElement,
      currentStep,
      depots,
      customers,
      routes,
      elements,
      gaSteps,
      metrics,
      setActiveElement,
      generatePath,
      randomPosition
    };
  }
};
</script>

<style>
.animate-dash {
  stroke-dasharray: 5;
  animation: dash 1s linear infinite;
}

@keyframes dash {
  to {
    stroke-dashoffset: -10;
  }
}
</style>