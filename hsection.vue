<!-- AcademicHeader.vue -->
<template>
  <header class="relative mb-12 pt-8 px-6">
    <!-- Top accent bar -->
    <div class="absolute top-0 left-1/2 transform -translate-x-1/2 w-24 h-1 bg-gradient-to-r from-primary-400 via-primary-600 to-primary-400"></div>

    <!-- Background highlights -->
    <div class="absolute inset-0 overflow-hidden">
      <!-- Top right highlight -->
      <div class="absolute -top-10 -right-10 w-40 h-40 bg-gradient-to-br from-primary-100/20 to-transparent rounded-full blur-2xl"></div>
      <!-- Bottom left highlight -->
      <div class="absolute -bottom-10 -left-10 w-40 h-40 bg-gradient-to-tr from-primary-100/20 to-transparent rounded-full blur-2xl"></div>
    </div>

    <div class="text-center relative interactive" >
      <!-- Main heading group -->
      <div class="relative inline-block">
        <!-- Accent dots -->
        <div class="absolute -left-6 top-1/2 transform -translate-y-1/2 flex flex-col gap-1.5">
          <div class="w-1.5 h-1.5 rounded-full bg-primary-400"></div>
          <div class="w-1.5 h-1.5 rounded-full bg-primary-300"></div>
          <div class="w-1.5 h-1.5 rounded-full bg-primary-200"></div>
        </div>

        <div class="absolute -right-6 top-1/2 transform -translate-y-1/2 flex flex-col gap-1.5">
          <div class="w-1.5 h-1.5 rounded-full bg-primary-200"></div>
          <div class="w-1.5 h-1.5 rounded-full bg-primary-300"></div>
          <div class="w-1.5 h-1.5 rounded-full bg-primary-400"></div>
        </div>

        <!-- Main heading -->
        <h1 class="text-4xl font-bold text-gray-800 mb-4 relative tracking-tight">
          <span class="relative inline-block px-2">
            Academic Progress Dashboard

            <!-- Highlight effect -->
            <span class="absolute inset-0 bg-gradient-to-r from-primary-50 via-primary-100/50 to-primary-50 rounded-lg -z-10"></span>

            <!-- Corner accents -->
            <span class="absolute top-0 left-0 w-2 h-2 border-t-2 border-l-2 border-primary-400"style="border-color: #939998;"></span>
            <span class="absolute top-0 right-0 w-2 h-2 border-t-2 border-r-2 border-primary-400" style="border-color: #939998;"></span>
            <span class="absolute  left-0 w-2 h-2 border-b-2 border-l-2 border-primary-400"style="
bottom: -6px ;border-color: #939998;" ></span>
            <span class="absolute  right-0 w-2 h-2 border-b-2 border-r-2 border-primary-400" style="
bottom: -6px;border-color: #939998;"></span>
          </span>
        </h1>
      </div>

      <!-- Subheading with metadata -->
      <div class="inline-flex items-center bg-white/50 backdrop-blur-sm px-6 py-2 rounded-full shadow-sm border border-gray-100" style="margin-left: 25px;">
        <div class="flex items-center space-x-6">
          <!-- Academic Year -->
          <div class="flex items-center">
            <div class="w-8 h-8 rounded-full bg-primary-100 flex items-center justify-center mr-3">
              <svg class="w-4 h-4 text-primary-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                      d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
              </svg>
            </div>
            <div class="flex flex-col items-start">
              <span class="text-xs text-gray-500">Academic Year</span>
              <span class="font-medium text-gray-800">2024</span>
            </div>
          </div>

          <!-- Divider -->
          <div class="h-8 w-px bg-gray-200"></div>

          <!-- Semester -->
          <div class="flex items-center">
            <div class="w-8 h-8 rounded-full bg-primary-100 flex items-center justify-center mr-3">
              <svg class="w-4 h-4 text-primary-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                      d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
              </svg>
            </div>
            <div class="flex flex-col items-start">
              <span class="text-xs text-gray-500">Current Term</span>
              <span class="font-medium text-gray-800">Semester 1</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AcademicHeader',
}
</script>

<style scoped>
/* Ensure text remains sharp */
* {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Subtle hover effect for interactive elements */
.interactive {
  transition: all 0.2s ease-in-out;
}

.interactive:hover {
  transform: translateY(-1px);
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-5px); }
}

.float {
  animation: float 6s ease-in-out infinite;
}
</style>