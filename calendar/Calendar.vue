<!-- Calendar.vue (Main Component) -->
<template>
  <div class="max-w-6xl mx-auto bg-slate-50 rounded-xl shadow-lg p-6" style="font-family: poppins,serif
">
    <CalendarHeader
        :week-start="weekStartDate"
        :view-size="viewSize"
        @update:view-size="viewSize = $event"
        @previous-week="previousWeek"
        @next-week="nextWeek"
    />

    <WeeklyStats :stats="weeklyStats" />

    <SubjectLegend
        :colors="subjectColors"
        :filtered-subjects="filteredSubjects"
        :stats="subjectStats"
        @toggle-subject="toggleSubjectFilter"
    />

    <ScheduleGrid
        :week-days="weekDays"
        :display-hours="displayHours"
        :items="scheduleItems"
        :view-size="viewSize"
        :colors="subjectColors"
        :filtered-subjects="filteredSubjects"
        @select-item="selectedItem = $event"
    />

    <ItemDetailsModal
        :item="selectedItem"
        @close="selectedItem = null"
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import CalendarHeader from './CalendarHeader.vue'
import WeeklyStats from './WeeklyStats.vue'
import SubjectLegend from './SubjectLegend.vue'
import ScheduleGrid from './ScheduleGrid.vue'
import ItemDetailsModal from './ItemDetailsModal.vue'

// State
const viewSize = ref('normal')
const selectedItem = ref(null)
const currentDate = ref(new Date())
const filteredSubjects = ref(['Math', 'Physics', 'Chemistry', 'Biology'])

// Colors configuration
const subjectColors = {
  Math: {
    bg: 'bg-blue-100 text-blue-800',
    dot: 'bg-blue-500'
  },
  Physics: {
    bg: 'bg-purple-100 text-purple-800',
    dot: 'bg-purple-500'
  },
  Chemistry: {
    bg: 'bg-green-100 text-green-800',
    dot: 'bg-green-500'
  },
  Biology: {
    bg: 'bg-orange-100 text-orange-800',
    dot: 'bg-orange-500'
  }
}

// Computed properties
const weekStartDate = computed(() => {
  const date = new Date(currentDate.value)
  const day = date.getDay()
  date.setDate(date.getDate() - day)
  return date
})

const weekDays = computed(() => {
  const days = []
  const start = new Date(weekStartDate.value)
  for (let i = 0; i < 7; i++) {
    const day = new Date(start)
    day.setDate(start.getDate() + i)
    days.push(day)
  }
  return days
})

const displayHours = computed(() => {
  return Array.from({ length: 14 }, (_, i) => i + 8) // 8 AM to 9 PM
})

// Sample schedule items (in real app, this would likely come from props or API)
const scheduleItems = [
  {
    id: 1,
    subject: 'Biology',
    type: 'meeting',
    startHour: 11.5,
    endHour: 12.5,
    day: new Date(weekStartDate.value.setDate(weekStartDate.value.getDate())),
    location: 'tunis khaznadar',
    professor: 'Dr. Smith',
    description: 'Advanced Calculus',
    materials: ['Textbook Ch. 5', 'Calculator'],
    assignments: ['Problem Set 3']
  },
  {
    id:2 ,
    subject: 'Math',
    type: 'meeting',
    startHour: 9,
    endHour: 10.5,
    day: new Date(weekStartDate.value.setDate(weekStartDate.value.getDate())),
    location: 'tunis ibn khaldoun',
    professor: 'Dr. Smith',
    description: 'Advanced Calculus',
    materials: ['Textbook Ch. 5', 'Calculator'],
    assignments: ['Problem Set 3']
  },
  {
    id:3 ,
    subject: 'Chemistry',
    type: 'meeting',
    startHour: 14,
    endHour: 16,
    day: "Sun Nov 12 2024 08:27:31 GMT+0100 (heure normale d’Afrique de l’Ouest)",
    location: 'tunis ibn khaldoun',
    professor: 'Dr. Smith',
    description: 'Advanced Calculus',
    materials: ['Textbook Ch. 5', 'Calculator'],
    assignments: ['Problem Set 3']
  }
  // Add more items as needed
]

// Weekly statistics
const weeklyStats = computed(() => [
  {
    label: 'Total Classes',
    value: scheduleItems.filter(item => item.type === 'class').length
  },
  {
    label: 'Total Hours',
    value: scheduleItems.reduce((acc, item) => acc + (item.endHour - item.startHour), 0).toFixed(1)
  },
  {
    label: 'Meetings',
    value: scheduleItems.filter(item => item.type === 'meeting').length
  },
  {
    label: 'Assignments Due',
    value: scheduleItems.reduce((acc, item) => acc + (item.assignments?.length || 0), 0)
  }
])

// Subject statistics
const subjectStats = computed(() => {
  const stats = {}
  Object.keys(subjectColors).forEach(subject => {
    const subjectItems = scheduleItems.filter(item => item.subject === subject)
    stats[subject] = `${subjectItems.length} classes`
  })
  return stats
})

// Methods
const previousWeek = () => {
  const newDate = new Date(currentDate.value)
  newDate.setDate(newDate.getDate() - 7)
  currentDate.value = newDate
}

const nextWeek = () => {
  const newDate = new Date(currentDate.value)
  newDate.setDate(newDate.getDate() + 7)
  currentDate.value = newDate
}

const toggleSubjectFilter = (subject) => {
  if (filteredSubjects.value.includes(subject)) {
    filteredSubjects.value = filteredSubjects.value.filter(s => s !== subject)
  } else {
    filteredSubjects.value.push(subject)
  }
}
</script>