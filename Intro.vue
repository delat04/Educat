<template>
  <div class="max-w-4xl mx-auto p-6 space-y-8">
    <!-- Header -->
    <div class="space-y-4">
      <h1 class="text-3xl font-bold text-gray-900">
        Multiple Depot Vehicle Routing Problem (MDVRP)
      </h1>
      <p class="text-lg text-gray-600">
        An advanced logistics optimization problem that involves routing vehicles from multiple depots to serve a set of customers efficiently.
      </p>
    </div>

    <!-- Problem Description -->
    <div class="bg-white rounded-lg shadow-lg p-6">
      <h2 class="text-xl font-semibold mb-4 flex items-center gap-2">
        <TruckIcon class="text-blue-500" />
        Understanding MDVRP
      </h2>
      <div class="grid md:grid-cols-2 gap-6">
        <div class="space-y-4">
          <h3 class="font-medium text-gray-900">Key Components:</h3>
          <ul class="space-y-3">
            <li class="flex items-start gap-3">
              <BuildingIcon class="w-5 h-5 text-blue-500 mt-1 flex-shrink-0" />
              <span><strong>Multiple Depots:</strong> Starting points with limited capacity and vehicle fleets</span>
            </li>
            <li class="flex items-start gap-3">
              <UserIcon class="w-5 h-5 text-green-500 mt-1 flex-shrink-0" />
              <span><strong>Customers:</strong> Locations with specific demands that need to be served</span>
            </li>
            <li class="flex items-start gap-3">
              <TruckIcon class="w-5 h-5 text-purple-500 mt-1 flex-shrink-0" />
              <span><strong>Routes:</strong> Paths that vehicles take to serve customers from their assigned depot</span>
            </li>
          </ul>
        </div>
        <div class="space-y-4">
          <h3 class="font-medium text-gray-900">Constraints:</h3>
          <ul class="space-y-2 text-gray-600">
            <li>• Each customer must be served by exactly one vehicle</li>
            <li>• Each route starts and ends at the same depot</li>
            <li>• Total customer demand on a route cannot exceed vehicle capacity</li>
            <li>• Each depot has a limited capacity and fleet size</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Genetic Algorithm Explanation -->
    <div class="bg-white rounded-lg shadow-lg p-6">
      <h2 class="text-xl font-semibold mb-4 flex items-center gap-2">
        <DnaIcon class="text-blue-500" />
        Genetic Algorithm Solution
      </h2>
      <div class="space-y-6">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
          <div
              v-for="(step, index) in steps"
              :key="index"
              :class="`p-4 rounded-lg border ${step.color}`"
          >
            <h3 class="font-semibold mb-2">{{ step.title }}</h3>
            <p class="text-sm text-gray-600">{{ step.description }}</p>
          </div>
        </div>

        <div class="bg-gray-50 p-4 rounded-lg">
          <h3 class="font-medium mb-2">Fitness Evaluation:</h3>
          <ul class="space-y-2 text-sm text-gray-600">
            <li>• Total distance traveled across all routes</li>
            <li>• Balance of workload between depots</li>
            <li>• Adherence to capacity constraints</li>
            <li>• Number of vehicles used</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Tips Section -->
    <div class="bg-blue-50 rounded-lg p-6">
      <h2 class="text-lg font-semibold mb-3">💡 Key Insights:</h2>
      <ul class="space-y-2 text-gray-700">
        <li>• The algorithm continuously improves solutions over multiple generations</li>
        <li>• Better initial solutions lead to faster convergence</li>
        <li>• Mutation rate affects exploration vs exploitation balance</li>
        <li>• Solution quality improves with population size but increases computation time</li>
      </ul>
    </div>
  </div>
</template>

<script>
import { Truck as TruckIcon, Building as BuildingIcon, User as UserIcon, Dna as DnaIcon } from 'lucide-vue-next';

export default {
  name: 'Introduction',
  components: {
    TruckIcon,
    BuildingIcon,
    UserIcon,
    DnaIcon
  },
  data() {
    return {
      steps: [
        {
          title: "1. Initialization",
          description: "Create initial population of random routes, assigning customers to depots while respecting constraints",
          color: "bg-blue-50 border-blue-200"
        },
        {
          title: "2. Selection",
          description: "Choose the fittest solutions based on total distance and load distribution",
          color: "bg-green-50 border-green-200"
        },
        {
          title: "3. Crossover",
          description: "Combine good solutions to create new offspring routes",
          color: "bg-purple-50 border-purple-200"
        },
        {
          title: "4. Mutation",
          description: "Introduce small random changes to maintain diversity",
          color: "bg-orange-50 border-orange-200"
        }
      ]
    };
  }
};
</script>

<style>
/**** Add global styles here if needed ****/
</style>
