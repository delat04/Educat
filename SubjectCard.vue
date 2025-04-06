<template>
  <div class="relative bg-white p-8 rounded-2xl shadow-lg mb-6 border-l-4 border-gray-200 transition-all duration-300 hover:shadow-xl"
        style="border-top: 1px solid rgb(229 231 235)">
    <!-- Academic Status Badge -->
    <div class="absolute top-4 right-4 px-3 py-1 rounded-full text-xs font-semibold"
         :class="[badgeBgColorClass, textColorClass]">
      {{ academicStatus }}
    </div>

    <div class="flex items-start space-x-8">
      <!-- Left Column: Icon and Basic Info -->
      <div class="flex flex-col items-center space-y-4">
        <!-- Icon Container with Custom Background -->
        <div class="relative group">
          <div class="flex items-center justify-center w-48 h-48 rounded-2xl transition-transform duration-300 group-hover:scale-105 shadow-md"
               :class="[iconBgColorClass, borderColorClass]"
               style="border-width: 1px; width: 14rem; height: 14rem">
            <img :src="iconClass" class="w-24 h-24 object-contain" :alt="subjectName" />
          </div>
        </div>

        <!-- Academic Level Indicator -->
        <div class="flex items-center space-x-1">
          <div v-for="i in 5" :key="i"
               class="w-2 h-2 rounded-full transition-all duration-300"
               :class="i <= difficultyLevel ? indicatorColorClass : 'bg-gray-200'">
          </div>
        </div>
        <span class="text-xs font-medium text-gray-500">{{ difficultyText }}</span>
      </div>

      <!-- Right Column: Main Content -->
      <div class="flex-1 space-y-6">
        <!-- Header -->
        <div>
          <h2 class="text-2xl font-bold text-gray-800 mb-2">{{ subjectName }}</h2>
          <p class="text-gray-600 text-sm leading-relaxed">{{ description }}</p>
        </div>

        <!-- Progress Section -->
        <div class="space-y-3">
          <div class="flex justify-between items-center">
            <span class="text-sm font-medium text-gray-600">Course Progress</span>
            <span class="text-sm font-bold" :class="textColorClass">{{ progress }}%</span>
          </div>
          <div class="relative w-full h-2 bg-gray-100 rounded-full overflow-hidden">
            <div class="absolute top-0 left-0 h-full rounded-full transition-all duration-500"
                 :class="indicatorColorClass"
                 :style="{ width: `${progress}%` }">
            </div>
          </div>
        </div>

        <!-- Stats Grid -->
        <div class="grid grid-cols-3 gap-4 pt-4">
          <!-- Time Spent -->
          <div class="p-4 rounded-xl bg-gray-50 border border-gray-100 hover:shadow-md transition-all duration-300">
            <div class="text-xs text-gray-500 mb-1">Time Invested</div>
            <div class="flex items-baseline space-x-1">
              <span class="text-lg font-bold" :class="textColorClass">{{ hours }}</span>
              <span class="text-sm font-medium text-gray-600">hrs</span>
              <span class="text-lg font-bold" :class="textColorClass">{{ paddedMinutes }}</span>
              <span class="text-sm font-medium text-gray-600">min</span>
            </div>
          </div>

          <!-- Next Assignment -->
          <div class="p-4 rounded-xl bg-gray-50 border border-gray-100 hover:shadow-md transition-all duration-300">
            <div class="text-xs text-gray-500 mb-1">Next Due</div>
            <div class="font-medium text-gray-800">{{ nextAssignment }}</div>
          </div>

          <!-- Grade/Score -->
          <div class="p-4 rounded-xl bg-gray-50 border border-gray-100 hover:shadow-md transition-all duration-300">
            <div class="text-xs text-gray-500 mb-1">Current Grade</div>
            <div class="font-bold text-lg" :class="textColorClass">{{ currentGrade }}</div>
          </div>
        </div>

        <!-- Additional Interactive Elements -->
        <div class="flex space-x-4 pt-2">
          <button class="px-4 py-2 text-sm font-medium rounded-lg transition-colors duration-300"
                  :class="[badgeBgColorClass, textColorClass]">
            View Details
          </button>
          <button class="px-4 py-2 text-sm font-medium text-gray-600 rounded-lg transition-colors duration-300 hover:bg-gray-100">
            Resources
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AcademicSubjectCard",
  props: {
    subjectName: String,
    description: String,
    iconClass: String,
    iconBgColor: { type: String, default: "blue-500" },
    progressColor: { type: String, default: "blue-500" },
    progress: { type: [Number, String], default: 0 },
    timeSpent: { type: String, default: "0 hours 0 min" },
    difficultyLevel: { type: Number, default: 3, validator: value => value >= 1 && value <= 5 },
    currentGrade: { type: String, default: "A" },
    nextAssignment: { type: String, default: "Final Exam" },
    academicStatus: { type: String, default: "On Track" }
  },
  computed: {
    colorClasses() {
      const colorMap = {
        "blue-500": "text-blue-500 bg-blue-500 border-blue-500",
        "purple-500": "text-purple-500 bg-purple-500 border-purple-500",
        "green-600": "text-green-600 bg-green-600 border-green-600",
        "cyan-600": "text-cyan-600 bg-cyan-600 border-cyan-600",
        "orange-600": "text-orange-600 bg-orange-600 border-orange-600",
        "rose-600": "text-rose-600 bg-rose-600 border-rose-600",
        "violet-600": "text-violet-600 bg-violet-600 border-violet-600",
        "teal-600": "text-teal-600 bg-teal-600 border-teal-600"
      };
      return colorMap[this.progressColor] || "text-blue-500 bg-blue-500 border-blue-500";
    },
    borderColorClass() {
      return this.colorClasses.split(" ").find(cls => cls.startsWith("border"));
    },
    textColorClass() {
      return this.colorClasses.split(" ").find(cls => cls.startsWith("text"));
    },
    badgeBgColorClass() {
      return this.colorClasses.split(" ").find(cls => cls.startsWith("bg")) + "/10";
    },
    iconBgColorClass() {
      return this.colorClasses.split(" ").find(cls => cls.startsWith("bg"));
    },
    indicatorColorClass() {
      return this.colorClasses.split(" ").find(cls => cls.startsWith("bg"));
    },
    hours() {
      return this.timeSpent.split(" ")[0];
    },
    paddedMinutes() {
      const minutes = this.timeSpent.split(" ")[2];
      return minutes.padStart(2, "0");
    },
    difficultyText() {
      const levels = { 1: "Introductory", 2: "Basic", 3: "Intermediate", 4: "Advanced", 5: "Expert" };
      return levels[this.difficultyLevel];
    }
  }
};
</script>

<style scoped>
.transition-all {
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 300ms;
}

.hover\:shadow-xl:hover {
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1),
  0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.group:hover .group-hover\:scale-105 {
  transform: scale(1.05);
}
</style>