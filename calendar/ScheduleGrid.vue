<!-- ScheduleGrid.vue -->
<template>
  <div class="overflow-x-auto bg-white rounded-lg shadow-sm">
    <div class="min-w-[1000px]">
      <!-- Hours Header -->
      <div class="grid grid-cols-[120px_repeat(14,minmax(70px,1fr))] border-b border-slate-200 sticky top-0 bg-white z-10">
        <div class="px-3 py-3 font-medium text-slate-500">Days</div>
        <div v-for="hour in displayHours" :key="hour"
             class="px-2 py-3 text-center text-sm font-medium text-slate-500">
          {{ formatHour(hour) }}
        </div>
      </div>

      <!-- Days and Schedule -->
      <div class="relative">
        <div v-for="(day, index) in weekDays" :key="day"
             class="grid grid-cols-[120px_repeat(14,minmax(70px,1fr))] border-b border-slate-200"
             :class="index % 2 === 0 ? 'bg-slate-50' : ''">
          <DayColumn :day="day" />
          <TimeSlot
              v-for="hour in displayHours"
              :key="`${day}-${hour}`"
              :day="day"
              :hour="hour"
              :view-size="viewSize"
              :items="getFilteredItems(day, hour)"
              :colors="colors"
              @select-item="$emit('select-item', $event)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import DayColumn from './DayColumn.vue'
import TimeSlot from './TimeSlot.vue'

const props = defineProps({
  weekDays: {
    type: Array,
    required: true
  },
  displayHours: {
    type: Array,
    required: true
  },
  items: {
    type: Array,
    required: true
  },
  viewSize: {
    type: String,
    required: true
  },
  colors: {
    type: Object,
    required: true
  },
  filteredSubjects: {
    type: Array,
    required: true
  }
})

defineEmits(['select-item'])

const getFilteredItems = (day, hour) => {
  return props.items.filter(item => {
    const itemDate = new Date(item.day)
    return itemDate.getDate() === day.getDate() &&
        itemDate.getMonth() === day.getMonth() &&
        item.startHour <= hour + 1 &&
        item.endHour >= hour &&
        props.filteredSubjects.includes(item.subject)
  })
}

const formatHour = (hour) => {
  return `${hour % 12 || 12}${hour >= 12 ? 'PM' : 'AM'}`
}
</script>