<!-- AcademicDashboard.vue -->
<template>
  <div class=" min-h-screen py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-6xl mx-auto">
      <!-- Header Section -->
      <AcademicHeader />

      <div class="bg-white academic-pattern rounded-2xl shadow-xl p-8 gradient-bg">
        <div class="flex flex-col lg:flex-row gap-8">
          <!-- Left Side (Cards) -->
          <div class="flex-1 grid grid-cols-1 sm:grid-cols-2 gap-6">
            <StatCard
                :stats="{ value: 14, label: 'Day Streak' }"
                theme="darksalmon"
                icon="🔥"
                :dots="5"
            />
            <StatCard
                :stats="{ value: 393, label: 'Exercises Completed' }"
                theme="cadetblue"
                icon="📚"
                increase="5"
            />
            <StatCard
                :stats="{ value: 12, label: 'Exams Taken' }"
                theme="cornflowerblue"
                icon="📝"
                increase="17"
            />
            <StatCard
                :stats="{ value: 141, label: 'Lessons Completed' }"
                theme="hotpink"
                icon="🎓"
                increase="2"
            />
          </div>

          <!-- Right Side (Chart) -->
          <div class="lg:w-2/5">
            <div class="bg-gradient-to-br from-white to-white-100 rounded-xl p-6 shadow-xl h-full card-hover relative overflow-hidden">

              <div class="achievement-badge">📊</div>
              <div class="relative">
                <h2 class="text-2xl font-bold text-teal-900 mb-4">Study Time Distribution</h2>
                <p class="text-sm font-medium text-gray-700 mb-6">
                  Total study time:
                  <span class="font-bold text-teal-800">{{ totalStudyTime }}</span>
                </p>
                <div class="relative h-64 mb-6">
                  <canvas ref="timeChart"></canvas>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue'
import Chart from 'chart.js/auto'
import StatCard from './StatCard.vue'
import AcademicHeader from "@/components/Educat/hsection.vue";

export default defineComponent({
  name: 'AcademicDashboard',
  components: {
    AcademicHeader,
    StatCard
  },
  setup() {
    const timeChart = ref(null)
    const totalStudyTime = ref('35 hrs 21 mins')

    const chartData = {
      labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      datasets: [{
        label: 'Study Hours',
        data: [4.5, 6.2, 5.8, 7.1, 5.4, 3.2, 3.1],
        borderColor: '#0d9488',
        backgroundColor: 'rgba(13, 148, 136, 0.1)',
        tension: 0.4,
        fill: true
      }]
    }

    onMounted(() => {
      const ctx = timeChart.value.getContext('2d')
      new Chart(ctx, {
        type: 'line',
        data: chartData,
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              display: false
            }
          },
          scales: {
            y: {
              beginAtZero: true,
              grid: {
                display: true,
                color: 'rgba(0,0,0,0.05)'
              }
            },
            x: {
              grid: {
                display: false
              }
            }
          }
        }
      })
    })

    return {
      timeChart,
      totalStudyTime
    }
  }
})
</script>




<style scoped>
.academic-pattern {
  background-image:
      linear-gradient(rgba(0, 0, 23, 0.8), rgba(255, 255, 255, 0.8)),
      url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23000000' fill-opacity='0.05'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
}

.gradient-bg {
  background: linear-gradient(135deg, #f8fafc 0%, #ffffff 100%);
  position: relative;
  overflow: hidden;
}

.gradient-bg::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(45deg, rgba(59, 130, 246, 0.05), rgba(16, 185, 129, 0.05));
  pointer-events: none;
}

.achievement-badge {
  position: absolute;
  top: -20px;
  right: -20px;
  font-size: 3rem;
  transform: rotate(45deg);
  opacity: 0.1;
}

</style>