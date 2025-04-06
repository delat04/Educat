<!-- AcademicStats.vue -->
<template>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-6 p-6">
    <!-- Completed Courses Card -->
    <div class="relative bg-gradient-to-br from-blue-500 to-violet-600 rounded-xl shadow-lg overflow-hidden card-hover h-[300px]">
      <!-- Abstract SVG Background -->
      <svg class="absolute inset-0 w-full h-full opacity-15" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="currentColor" d="M42.7,-73.2C55.9,-67.3,67.7,-57.1,75.6,-44.2C83.4,-31.3,87.3,-15.7,86.9,-0.2C86.5,15.2,81.8,30.4,73.7,43.2C65.6,56,54.1,66.4,41,72.5C27.9,78.6,13.9,80.5,-0.7,81.7C-15.3,82.9,-30.6,83.5,-43.8,77.4C-57,71.3,-68,58.5,-76.4,44.1C-84.8,29.7,-90.6,14.9,-89.9,0.4C-89.2,-14,-82,-28,-73.1,-39.7C-64.2,-51.4,-53.6,-60.8,-41.4,-67.4C-29.2,-74,-14.6,-77.9,0.3,-78.4C15.2,-78.9,30.4,-76,42.7,-73.2Z" transform="translate(100 100)" />
      </svg>
      <div class="relative p-6 flex flex-col h-full text-white">
        <div class="flex items-center justify-between mb-4">
          <h3 class="text-lg font-semibold">Completed Courses</h3>
          <svg class="h-8 w-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20" />
            <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z" />
          </svg>
        </div>
        <div class="mt-auto">
          <div class="text-4xl font-bold mb-2">{{ courseStats.completed }}</div>
          <div class="text-white/80">Courses Completed</div>
          <div class="mt-4 bg-white/20 rounded-lg p-3">
            <div class="flex items-center">
              <span class="text-sm">
                {{ courseStats.growthRate >= 0 ? '↑' : '↓' }}
                {{ Math.abs(courseStats.growthRate) }}% from last semester
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Study Hours Card -->
    <div class="relative bg-gradient-to-br from-emerald-500 to-teal-600 rounded-xl shadow-lg overflow-hidden card-hover h-[300px]">
      <!-- Abstract SVG Background -->
      <svg class="absolute inset-0 w-full h-full opacity-15" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="currentColor" d="M39.9,-65.7C53.3,-59.6,67,-51.3,76.2,-39.1C85.4,-26.9,90.2,-10.8,88.8,4.8C87.4,20.4,79.9,35.5,69.7,47.7C59.5,59.9,46.7,69.2,32.5,74.3C18.3,79.4,2.7,80.4,-13.2,78.5C-29.1,76.7,-45.3,72,-57.8,62C-70.3,52,-79.1,36.7,-83.1,20.1C-87.1,3.5,-86.3,-14.4,-79.9,-29.3C-73.5,-44.2,-61.4,-56.1,-47.4,-62C-33.3,-67.9,-17.3,-67.8,-1.9,-64.8C13.5,-61.8,26.5,-71.8,39.9,-65.7Z" transform="translate(100 100)" />
      </svg>
      <div class="relative p-6 flex flex-col h-full text-white">
        <div class="flex items-center justify-between mb-4">
          <h3 class="text-lg font-semibold">Study Hours</h3>
          <svg class="h-8 w-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10" />
            <polyline points="12 6 12 12 16 14" />
          </svg>
        </div>
        <div class="mt-auto">
          <div class="text-4xl font-bold mb-2">{{ studyStats.totalHours }}</div>
          <div class="text-white/80">Total Hours</div>
          <div class="mt-4">
            <div class="w-full bg-white/20 rounded-full h-2.5 mb-2">
              <div
                  class="bg-white h-2.5 rounded-full transition-all duration-500"
                  :style="{ width: `${studyStats.progressPercentage}%` }"
              ></div>
            </div>
            <div class="text-sm text-white/80">{{ studyStats.progressPercentage }}% of goal</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Academic Performance Card -->
    <div class="relative bg-gradient-to-br from-amber-500 to-orange-600 rounded-xl shadow-lg overflow-hidden card-hover h-[300px]">
      <!-- Abstract SVG Background -->
      <svg class="absolute inset-0 w-full h-full opacity-15" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="currentColor" d="M44.7,-76.4C58.8,-69.2,71.8,-59,79.7,-45.8C87.6,-32.6,90.3,-16.3,89.4,-0.5C88.5,15.3,84,30.5,76.2,43.5C68.4,56.5,57.3,67.2,43.7,74.6C30.1,82,15.1,86.1,0.7,85C-13.7,83.9,-27.4,77.6,-40.9,70C-54.4,62.4,-67.7,53.4,-77.4,41C-87.1,28.5,-93.2,14.3,-92.8,0.2C-92.4,-13.8,-85.5,-27.6,-76.7,-39.2C-67.9,-50.8,-57.2,-60.2,-44.7,-68.1C-32.2,-76,-16.1,-82.4,-0.1,-82.2C15.9,-82,31.8,-75.2,44.7,-76.4Z" transform="translate(100 100)" />
      </svg>
      <div class="relative p-6 flex flex-col h-full text-white">
        <div class="flex items-center justify-between mb-4">
          <h3 class="text-lg font-semibold">Academic Score</h3>
          <svg class="h-8 w-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M12 15l-8.5 8.5L7 19l5-5 5 5 3.5-3.5L12 7" />
            <path d="M12 15V3" />
          </svg>
        </div>
        <div class="mt-auto">
          <div class="text-4xl font-bold mb-2">{{ academicStats.gpa.toFixed(1) }}</div>
          <div class="text-white/80">Current GPA</div>
          <div class="flex flex-wrap gap-2 mt-4">
            <span v-for="(achievement, index) in academicStats.achievements"
                  :key="index"
                  class="px-3 py-1 text-xs rounded-full bg-white/20 backdrop-blur-sm">
              {{ achievement.text }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AcademicStats',
  data() {
    return {
      courseStats: {
        completed: 12,
        growthRate: 8,
        total: 15
      },
      studyStats: {
        totalHours: 186,
        goalHours: 248,
        progressPercentage: 75
      },
      academicStats: {
        gpa: 3.8,
        achievements: [
          { text: "Dean's List" },
          { text: 'Top 10%' },
          { text: 'Honor Roll' }
        ]
      }
    }
  },
  methods: {
    updateStudyProgress() {
      this.studyStats.progressPercentage = Math.floor(
          (this.studyStats.totalHours / this.studyStats.goalHours) * 100
      )
    }
  },
  mounted() {
    this.updateStudyProgress()
  }
}
</script>

<style scoped>
.card-hover {
  transition: all 0.3s ease;
}

.card-hover:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2), 0 10px 10px -5px rgba(0, 0, 0, 0.1);
}

.card-hover:hover .opacity-15 {
  opacity: 0.25;
  animation: float 6s ease-in-out infinite;
}

@keyframes float {
  0%, 100% {
    transform: translate(100px, 100px) scale(1);
  }
  50% {
    transform: translate(100px, 100px) scale(1.05);
  }
}
</style>