<!-- TimeSlot.vue -->
<template>
  <div
      class="relative transition-all duration-200"
      :class="[slotHeightClass, 'hover:bg-gray-50 dark:hover:bg-gray-800/30']"
      role="grid"
      aria-label="Time slot container"
  >
    <template v-for="item in items" :key="item.id">
      <div
          @click="handleItemSelection(item)"
          @keydown.enter="handleItemSelection(item)"
          class="absolute top-0 h-full rounded-lg mx-1 cursor-pointer transition-all"
          :class="[
          getItemClasses(item),
          'focus:outline-none focus:ring-2 focus:ring-offset-2'
        ]"
          :style="getItemStyle(item)"
          role="gridcell"
          tabindex="0"
          :aria-label="`${item.subject} from ${formatTimeRange(item.startHour, item.endHour)}`"
      >
        <div class="p-2 h-full flex flex-col justify-between">
          <div class="space-y-1">
            <div class="text-xs font-medium truncate tracking-wide">
              {{ item.subject }}
            </div>
            <div
                v-if="viewSize !== 'compact'"
                class="text-[10px] opacity-75 font-light"
            >
              {{ item.location }}
            </div>
          </div>
          <div
              v-if="viewSize === 'expanded'"
              class="text-[10px] mt-1 font-mono"
          >
            {{ formatTimeRange(item.startHour, item.endHour) }}
          </div>
        </div>
      </div>
    </template>
  </div>
</template>

<script setup>
import { computed } from 'vue'

/**
 * @typedef {Object} TimeSlotItem
 * @property {string} id - Unique identifier for the item
 * @property {string} subject - Subject or title of the time slot
 * @property {string} location - Location of the event
 * @property {number} startHour - Start hour (decimal format, e.g., 9.5 for 9:30)
 * @property {number} endHour - End hour (decimal format)
 * @property {string} type - Type of the item (e.g., 'meeting', 'task')
 */

/**
 * Component Props Definition
 */
const props = defineProps({
  hour: {
    type: Number,
    required: true,
    validator: (value) => value >= 0 && value <= 24
  },
  items: {
    type: Array,
    required: true,
    validator: (items) => items.every(item =>
        typeof item.startHour === 'number' &&
        typeof item.endHour === 'number' &&
        item.startHour <= item.endHour
    )
  },
  viewSize: {
    type: String,
    required: true,
    validator: (value) => ['compact', 'normal', 'expanded'].includes(value)
  },
  colors: {
    type: Object,
    required: true,
    validator: (value) => Object.values(value).every(color =>
        color && typeof color.bg === 'string'
    )
  }
})

const emit = defineEmits(['select-item'])

/**
 * Computed Properties
 */
const slotHeightClass = computed(() => ({
  'h-12': props.viewSize === 'compact',
  'h-16': props.viewSize === 'normal',
  'h-20': props.viewSize === 'expanded'
}))

/**
 * Utility Functions
 */
const getItemClasses = (item) => [
  props.colors[item.subject].bg,
  item.type === 'meeting' ? 'border-2 border-current' : '',
  'hover:transform hover:scale-[1.02]',
  'hover:shadow-lg transition-shadow duration-200',
  'hover:ring-2 hover:ring-offset-1 hover:ring-current'
]

const getItemStyle = (item) => {
  const startOffset = ((item.startHour % 1) * 100) + '%'
  const duration = Math.min(100, ((item.endHour - item.startHour)) * 100)
  return {
    left: startOffset,
    width: `${duration}%`,
    transition: 'all 0.2s ease-in-out'
  }
}

/**
 * Time Formatting Utilities
 */
const formatTimeRange = (start, end) => {
  const formatHour = (hour) => {
    const baseHour = hour % 12 || 12
    const meridiem = hour >= 12 ? 'PM' : 'AM'
    return `${baseHour}${meridiem}`
  }

  const formatMinutes = (decimal) => {
    const minutes = Math.round((decimal % 1) * 60)
    return minutes.toString().padStart(2, '0')
  }

  return `${formatHour(Math.floor(start))}:${formatMinutes(start)} - ${formatHour(Math.floor(end))}:${formatMinutes(end)}`
}

/**
 * Event Handlers
 */
const handleItemSelection = (item) => {
  emit('select-item', item)
}
</script>