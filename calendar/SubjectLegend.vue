<template>
  <section
      class="mb-6 p-6 bg-white rounded-xl shadow-sm transition-all duration-300 hover:shadow-md dark:bg-slate-800"
      aria-label="Subject filter controls" style="
    --tw-bg-opacity: 1;
    background-color: rgb(233 241 253);;
"
  >
    <div
        class="flex flex-wrap gap-6 "
        role="listbox"
        aria-label="Filter subjects"
    >
      <div
          v-for="(color, subject) in colors"
          :key="subject"
          role="option"
          :aria-selected="filteredSubjects.includes(subject)"
          tabindex="0"
          class="group relative flex items-center gap-3 cursor-pointer select-none"
          :class="[
          'transition-all duration-200 ease-in-out',
          'hover:transform hover:translate-y-[-2px]'
        ]"
          @click="handleSubjectToggle(subject)"
          @keydown.enter="handleSubjectToggle(subject)"
      >
        <!-- Subject Indicator -->
        <div
            class="relative w-4 h-4 rounded-full transition-all duration-200"
            :class="[
            color.dot,
            filteredSubjects.includes(subject) ? [
              'ring-2 ring-offset-2 dark:ring-offset-slate-800',
              'scale-110'
            ] : 'group-hover:scale-110',
            'after:absolute after:inset-0 after:rounded-full after:shadow-inner'
          ]"
        />

        <!-- Subject Information -->
        <div class="space-y-1">
          <div
              class="text-sm font-medium tracking-wide transition-colors"
              :class="[
              filteredSubjects.includes(subject)
                ? 'text-slate-900 dark:text-slate-100'
                : 'text-slate-700 dark:text-slate-300'
            ]" style="--tw-text-opacity: 1;
        color: rgb(67 67 67);"
          >
            {{ subject }}
          </div>
          <div
              class="text-xs font-light transition-colors"
              :class="[
              filteredSubjects.includes(subject)
                ? 'text-slate-600 dark:text-slate-400'
                : 'text-slate-500 dark:text-slate-500'
            ]"
          >
            {{ stats[subject] }}
          </div>
        </div>

        <!-- Selection Indicator -->
        <div
            v-if="filteredSubjects.includes(subject)"
            class="absolute -left-2 -translate-x-full opacity-0 group-hover:opacity-100 transition-opacity duration-200"
            aria-hidden="true"
        >
          <div class="text-sm text-slate-400">●</div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed } from 'vue'

/**
 * @typedef {Object} ColorConfig
 * @property {string} dot - Tailwind class for the dot color
 * @property {string} bg - Tailwind class for the background color
 */

/**
 * @typedef {Object} SubjectStats
 * @property {number} count - Number of items for the subject
 * @property {number} duration - Total duration in hours
 */

/**
 * Component Props Definition with TypeScript-style documentation
 */
const props = defineProps({
  /**
   * Color configurations for each subject
   * @type {Record<string, ColorConfig>}
   */
  colors: {
    type: Object,
    required: true,
    validator: (value) => {
      return Object.values(value).every(color =>
          typeof color.dot === 'string' && typeof color.bg === 'string'
      )
    }
  },

  /**
   * Currently selected subjects for filtering
   * @type {string[]}
   */
  filteredSubjects: {
    type: Array,
    required: true,
    validator: (value) => value.every(subject => typeof subject === 'string')
  },

  /**
   * Statistics for each subject
   * @type {Record<string, SubjectStats>}
   */
  stats: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle-subject'])

/**
 * Formats the statistics for display
 * @param {SubjectStats} stats - Statistics for a subject
 * @returns {string} Formatted statistics string
 */
const formatStats = (stats) => {
  if (!stats) return 'No data'

  const hours = Math.floor(stats.duration)
  const minutes = Math.round((stats.duration - hours) * 60)

  let timeStr = ''
  if (hours > 0) timeStr += `${hours}h `
  if (minutes > 0) timeStr += `${minutes}m`

  return `${stats.count} items · ${timeStr.trim()}`
}

/**
 * Handles the toggling of subject selection
 * @param {string} subject - The subject to toggle
 */
const handleSubjectToggle = (subject) => {
  emit('toggle-subject', subject)
}
</script>