<template>
  <Transition name="modal">
    <div v-if="item" class="fixed inset-0 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center z-50"
         @click.self="$emit('close')" >
      <div class="bg-white rounded-xl max-w-xl w-full mx-4 shadow-2xl overflow-hidden">
        <!-- Header -->
        <div class=" p-6 relative overflow-hidden" :class="subjectColors[item.subject]?.bg || 'bg-slate-100 text-slate-800'" style="
    font-family: 'Poppins',serif;
">
          <!-- Decorative waves SVG -->
          <svg class="absolute  left-0 w-full h-12 opacity-10" preserveAspectRatio="none" viewBox="0 0 1200 120" style="bottom:60px">
            <path d="M321.39,56.44c58-10.79,114.16-30.13,172-41.86,82.39-16.72,168.19-17.73,250.45-.39C823.78,31,906.67,72,985.66,92.83c70.05,18.48,146.53,26.09,214.34,3V0H0V27.35A600.21,600.21,0,0,0,321.39,56.44Z"
                  fill="currentColor" />
          </svg>
          <svg class="absolute  left-0 w-full h-16 opacity-5" preserveAspectRatio="none" viewBox="0 0 1200 120" style="bottom:41px">
            <path d="M0,0V46.29c47.79,22.2,103.59,32.17,158,28,70.36-5.37,136.33-33.31,206.8-37.5C438.64,32.43,512.34,53.67,583,72.05c69.27,18,138.3,24.88,209.4,13.08,36.15-6,69.85-17.84,104.45-29.34C989.49,25,1113-14.29,1200,52.47V0Z"
                  fill="currentColor" />
          </svg>

          <!-- Original content -->
          <div class="flex justify-between items-start relative z-10">
            <div>
              <span class="text-slate-400 text-sm">{{ item.type.toUpperCase() }}</span>
              <h3 class="text-2xl font-bold text-white mt-1">{{ item.subject }}</h3>
            </div>
            <button @click="$emit('close')"
                    class="text-slate-400 hover:text-white transition-colors">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                   class="w-6 h-6">
                <path d="M18 6L6 18M6 6l12 12" stroke-width="2" stroke-linecap="round"/>
              </svg>
            </button>
          </div>
        </div>

        <!-- Content -->
        <div class="p-6" style="
    font-family: 'Poppins',serif;
">
          <!-- Time and Location Section -->
          <div class="flex items-center gap-4 p-4 bg-slate-50 rounded-lg mb-6">
            <div class="flex-1">
              <div class="text-sm text-slate-500">Time</div>
              <div class="font-medium text-slate-800">{{ details.Time }}</div>
            </div>
            <div class="w-px h-12 bg-slate-200"></div>
            <div class="flex-1">
              <div class="text-sm text-slate-500">Location</div>
              <div class="font-medium text-slate-800">{{ details.Location }}</div>
            </div>
          </div>

          <!-- Professor Section -->
          <div v-if="details.Professor" class="mb-6">
            <div class="flex items-center gap-2 mb-2">
              <svg class="w-5 h-5 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
              </svg>
              <h4 class="font-semibold text-slate-700">Professor</h4>
            </div>
            <p class="text-slate-600 ml-7">{{ details.Professor }}</p>
          </div>

          <!-- Description Section -->
          <div v-if="details.Description" class="mb-6">
            <div class="flex items-center gap-2 mb-2">
              <svg class="w-5 h-5 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
              </svg>
              <h4 class="font-semibold text-slate-700">Description</h4>
            </div>
            <p class="text-slate-600 ml-7">{{ details.Description }}</p>
          </div>

          <!-- Materials Section -->
          <div v-if="details.Materials" class="mb-6">
            <div class="flex items-center gap-2 mb-2">
              <svg class="w-5 h-5 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
              </svg>
              <h4 class="font-semibold text-slate-700">Required Materials</h4>
            </div>
            <ul class="list-disc ml-12 text-slate-600">
              <li v-for="material in details.Materials.split(', ')"
                  :key="material">
                {{ material }}
              </li>
            </ul>
          </div>

          <!-- Assignments Section -->
          <div v-if="details.Assignments" class="mb-6">
            <div class="flex items-center gap-2 mb-2">
              <svg class="w-5 h-5 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4" />
              </svg>
              <h4 class="font-semibold text-slate-700">Assignments</h4>
            </div>
            <ul class="list-disc ml-12 text-slate-600">
              <li v-for="assignment in details.Assignments.split(', ')"
                  :key="assignment">
                {{ assignment }}
              </li>
            </ul>
          </div>

          <!-- Attendees Section -->
          <div v-if="details.Attendees" class="mb-6">
            <div class="flex items-center gap-2 mb-2">
              <svg class="w-5 h-5 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
              </svg>
              <h4 class="font-semibold text-slate-700">Attendees</h4>
            </div>
            <p class="text-slate-600 ml-7">{{ details.Attendees }}</p>
          </div>

          <!-- Action Buttons -->
          <div class="flex gap-3 mt-8">
            <button @click="$emit('close')"
                    class="flex-1 px-4 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-lg text-sm font-medium transition-colors">
              Close
            </button>
            <button v-if="item.type === 'meeting'"
                    class="flex-1 px-4 py-2.5   text-slate-400 rounded-lg text-sm font-medium transition-colors" :class="subjectColors[item.subject]?.bg  || 'bg-blue-600 hover:bg-blue-700'" >

              Join Meeting
            </button>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  item: {
    type: Object,
    default: null
  }
})
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

defineEmits(['close'])

const details = computed(() => {
  if (!props.item) return {}

  const formatTimeRange = (start, end) => {
    const formatHour = (hour) => `${hour % 12 || 12}${hour >= 12 ? 'PM' : 'AM'}`
    return `${formatHour(Math.floor(start))}:${(start % 1 * 60).toString().padStart(2, '0')} -
            ${formatHour(Math.floor(end))}:${(end % 1 * 60).toString().padStart(2, '0')}`
  }

  return {
    Time: formatTimeRange(props.item.startHour, props.item.endHour),
    Location: props.item.location,
    Type: props.item.type.charAt(0).toUpperCase() + props.item.type.slice(1),
    Professor: props.item.professor,
    Description: props.item.description,
    ...(props.item.materials && { Materials: props.item.materials.join(', ') }),
    ...(props.item.assignments && { Assignments: props.item.assignments.join(', ') }),
    ...(props.item.attendees && { Attendees: props.item.attendees.join(', ') })
  }
})
</script>

<style scoped>
.modal-enter-active,
.modal-leave-active {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

.modal-enter-to,
.modal-leave-from {
  opacity: 1;
  transform: scale(1);
}
</style>
