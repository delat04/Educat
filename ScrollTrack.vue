<template>
  <div class="relative min-h-screen flex">
    <div class="flex-1 pr-20">
      <!-- Main Content Sections -->
      <div class="sections space-y-4">
        <section
            v-for="(section, index) in sections"
            :key="index"
            :id="'section-' + index"
            class="min-h-screen p-8 border-b border-gray-200 relative bg-white transition-all duration-300 hover:bg-gray-50"
            ref="sectionsRef"
        >
          <div class="max-w-4xl mx-auto relative">
            <!-- Section Header -->
            <div class="section-number">{{ (index + 1).toString().padStart(2, "0") }}</div>

            <div class="flex items-center justify-between mb-6">
              <h2 class="text-3xl font-bold text-gray-800 relative z-10">
                {{ section.name }}
              </h2>
              <div class="flex items-center gap-3">
                <span class="estimated-time px-3 py-1 rounded-full text-sm bg-blue-100 text-blue-800">
                  {{ section.estimatedTime }} min
                </span>
                <div class="difficulty-badge" :class="getDifficultyClass(section.difficulty)">
                  {{ section.difficulty }}
                </div>
              </div>
            </div>

            <!-- Learning Objectives -->
            <div class="mb-6 bg-gray-50 p-4 rounded-lg">
              <h3 class="text-lg font-semibold text-gray-700 mb-2">Learning Objectives</h3>
              <ul class="space-y-2">
                <li v-for="(objective, idx) in section.objectives" :key="idx"
                    class="flex items-start gap-2">
                  <span class="mt-1 text-green-500">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                      <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                    </svg>
                  </span>
                  <span>{{ objective }}</span>
                </li>
              </ul>
            </div>

            <!-- Progress Bar -->
            <div class="progress-bar mb-6">
              <div
                  class="progress-fill"
                  :style="{ width: `${sectionProgress[index] || 0}%` }"
              ></div>
            </div>

            <!-- Main Content -->
            <div class="prose max-w-none">
              <div v-html="section.content"></div>
            </div>

            <!-- Interactive Elements -->
            <div class="mt-8 space-y-4">
              <div v-if="section.quiz" class="bg-blue-50 p-6 rounded-lg">
                <h3 class="text-xl font-semibold text-blue-900 mb-4">Quick Check</h3>
                <div v-for="(question, qIndex) in section.quiz" :key="qIndex" class="mb-4">
                  <p class="font-medium mb-2">{{ question.question }}</p>
                  <div class="space-y-2">
                    <label v-for="(option, oIndex) in question.options" :key="oIndex"
                           class="flex items-center p-3 bg-white rounded-lg cursor-pointer hover:bg-blue-100 transition-colors">
                      <input
                          type="radio"
                          :name="'question-' + qIndex"
                          :value="option"
                          v-model="quizAnswers[qIndex]"
                          class="mr-3"
                      >
                      {{ option }}
                    </label>
                  </div>
                </div>
              </div>
            </div>

            <!-- Section Stats -->
            <div class="mt-6 flex items-center justify-between text-sm text-gray-600">
              <div class="flex items-center gap-6">
                <span class="flex items-center gap-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-12a1 1 0 10-2 0v4a1 1 0 00.293.707l2.828 2.829a1 1 0 101.415-1.415L11 9.586V6z" clip-rule="evenodd" />
                  </svg>
                  {{ formatTime(timeSpent[index] || 0) }}
                </span>
                <span class="flex items-center gap-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                    <path d="M10 12a2 2 0 100-4 2 2 0 000 4z" />
                    <path fill-rule="evenodd" d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z" clip-rule="evenodd" />
                  </svg>
                  {{ sectionVisits[index] || 0 }} visits
                </span>
              </div>
              <button
                  @click="markAsComplete(index)"
                  class="px-4 py-2 rounded-lg text-sm font-medium transition-colors"
                  :class="isCompleted(index) ? 'bg-green-100 text-green-800' : 'bg-gray-100 text-gray-800 hover:bg-gray-200'"
              >
                {{ isCompleted(index) ? 'Completed' : 'Mark as Complete' }}
              </button>
            </div>
          </div>
        </section>
      </div>
    </div>

    <!-- Progress Tracker -->
    <div
        class="scroll-tracker transition-all duration-300"
        :class="{ 'w-64': !isCompact, 'w-12': isCompact }"
    >
      <button
          class="toggle-compact-btn"
          @click="isCompact = !isCompact"
      >
        {{ isCompact ? '›' : '‹' }}
      </button>

      <div class="flex flex-col gap-2">
        <div
            v-for="(section, index) in sections"
            :key="index"
            class="tracker-item group"
            :class="{
            'active': activeSection === index,
            'visited': sectionVisits[index] > 0
          }"
            @click="scrollToSection(index)"
        >
          <div class="progress-indicator"></div>
          <span v-show="!isCompact" class="ml-6 text-sm text-gray-600 truncate">
            {{ section.name }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue';

export default {
  setup() {
    // Utility function to load persisted state
    const loadPersistedState = (key, defaultValue) => {
      try {
        const saved = localStorage.getItem(key);
        return saved ? JSON.parse(saved) : defaultValue;
      } catch {
        return defaultValue;
      }
    };

    // Core state
    const sections = ref([
      {
        name: "Course Introduction",
        content: "Welcome to this enhanced learning experience. This course will guide you through the fundamentals and advanced concepts.",
        estimatedTime: 15,
        difficulty: "Beginner",
        objectives: [
          "Understand the course structure",
          "Set up your learning environment",
          "Review prerequisites"
        ],
        quiz: [
          {
            question: "What is the main goal of this course?",
            options: [
              "To learn web development",
              "To master Vue.js",
              "To understand modern JavaScript",
              "To build real-world applications"
            ],
            answer: 1
          }
        ]
      },
      {
        name: "Core Concepts",
        content: "Understanding the fundamental concepts is crucial for mastering this subject.",
        estimatedTime: 25,
        difficulty: "Intermediate",
        objectives: [
          "Master key theoretical concepts",
          "Apply concepts to real problems",
          "Complete practice exercises"
        ],
        quiz: [
          {
            question: "Which approach is most effective for learning?",
            options: [
              "Memorization only",
              "Practice and application",
              "Reading without practice",
              "Watching videos only"
            ],
            answer: 1
          }
        ]
      },
      {
        name: "Advanced Topics",
        content: "Explore advanced concepts and real-world applications.",
        estimatedTime: 35,
        difficulty: "Advanced",
        objectives: [
          "Understand advanced patterns",
          "Implement complex solutions",
          "Optimize performance"
        ],
        quiz: [
          {
            question: "What's the best way to approach complex problems?",
            options: [
              "Jump right in",
              "Break down into smaller parts",
              "Skip difficult parts",
              "Ask for complete solution"
            ],
            answer: 1
          }
        ]
      }
    ]);

    // State management
    const activeSection = ref(null);
    const sectionsRef = ref([]);
    const isCompact = ref(false);
    const tooltipIndex = ref(null);
    const tooltipStyle = ref({ top: '0px' });
    const scrollRequest = ref(null);
    const showLoadMore = ref(true);

    // Tracking state
    const sectionTimes = ref({});
    const timeSpent = ref(loadPersistedState('timeSpent', {}));
    const sectionVisits = ref(loadPersistedState('sectionVisits', {}));
    const sectionProgress = ref(loadPersistedState('sectionProgress', {}));
    const quizAnswers = ref(loadPersistedState('quizAnswers', {}));
    const completedSections = ref(new Set(loadPersistedState('completedSections', [])));

    // Progress tracking
    const calculateSectionProgress = () => {
      sectionsRef.value.forEach((section, index) => {
        if (!section) return;

        const rect = section.getBoundingClientRect();
        const viewHeight = window.innerHeight;
        const visibleHeight = Math.min(viewHeight, rect.bottom) - Math.max(0, rect.top);
        const progress = (visibleHeight / rect.height) * 100;

        sectionProgress.value[index] = Math.max(0, Math.min(100, progress));
      });
    };

    // Section management
    const updateActiveSection = () => {
      sectionsRef.value.forEach((section, index) => {
        if (!section) return;

        const rect = section.getBoundingClientRect();
        const midpoint = window.innerHeight / 2;

        if (rect.top <= midpoint && rect.bottom >= midpoint) {
          setActiveSection(index);
        }
      });
    };

    const setActiveSection = (index) => {
      if (activeSection.value !== index) {
        if (activeSection.value !== null) {
          logTime(activeSection.value);
        }
        activeSection.value = index;
        sectionTimes.value[index] = performance.now();
        sectionVisits.value[index] = (sectionVisits.value[index] || 0) + 1;
      }
    };

    // Time tracking
    const logTime = (index) => {
      if (sectionTimes.value[index]) {
        const timeSpent_sec = (performance.now() - sectionTimes.value[index]) / 1000;
        timeSpent.value[index] = (timeSpent.value[index] || 0) + timeSpent_sec;
      }
    };

    const formatTime = (seconds) => {
      const minutes = Math.floor(seconds / 60);
      const remainingSeconds = Math.floor(seconds % 60);
      return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`;
    };

    // Navigation
    const scrollToSection = (index) => {
      const section = sectionsRef.value[index];
      if (section) {
        section.scrollIntoView({ behavior: 'smooth' });
      }
    };

    // UI handlers
    const showTooltip = (index, event) => {
      tooltipIndex.value = index;
      tooltipStyle.value = {
        top: `${event.clientY}px`,
        [isCompact.value ? 'left' : 'right']: '45px'
      };
    };

    const hideTooltip = () => {
      tooltipIndex.value = null;
    };

    // Style utilities
    const getDifficultyClass = (difficulty) => {
      const classes = {
        'Beginner': 'bg-green-100 text-green-800',
        'Intermediate': 'bg-yellow-100 text-yellow-800',
        'Advanced': 'bg-red-100 text-red-800'
      };
      return `px-3 py-1 rounded-full text-sm ${classes[difficulty] || ''}`;
    };

    // Completion tracking
    const markAsComplete = (index) => {
      if (completedSections.value.has(index)) {
        completedSections.value.delete(index);
      } else {
        completedSections.value.add(index);
      }
      persistState();
    };

    const isCompleted = (index) => {
      return completedSections.value.has(index);
    };

    // Event handlers
    const handleScroll = () => {
      if (scrollRequest.value) return;

      scrollRequest.value = requestAnimationFrame(() => {
        calculateSectionProgress();
        updateActiveSection();
        scrollRequest.value = null;
      });
    };

    const handleResize = () => {
      calculateSectionProgress();
      updateActiveSection();
    };

    const handleKeydown = (event) => {
      if (['ArrowDown', 'ArrowUp'].includes(event.key)) {
        event.preventDefault();
        const direction = event.key === 'ArrowDown' ? 1 : -1;
        const newIndex = activeSection.value + direction;

        if (newIndex >= 0 && newIndex < sections.value.length) {
          scrollToSection(newIndex);
        }
      }
    };

    // Persistence
    const persistState = () => {
      localStorage.setItem('timeSpent', JSON.stringify(timeSpent.value));
      localStorage.setItem('sectionVisits', JSON.stringify(sectionVisits.value));
      localStorage.setItem('sectionProgress', JSON.stringify(sectionProgress.value));
      localStorage.setItem('quizAnswers', JSON.stringify(quizAnswers.value));
      localStorage.setItem('completedSections', JSON.stringify([...completedSections.value]));
    };

    // Initialization
    let observer = null;

    const initIntersectionObserver = () => {
      observer = new IntersectionObserver(
          (entries) => {
            entries.forEach((entry) => {
              if (entry.isIntersecting) {
                const index = parseInt(entry.target.id.split('-')[1]);
                setActiveSection(index);
              }
            });
          },
          {
            threshold: [0, 0.25, 0.5, 0.75, 1],
            rootMargin: '-50% 0px'
          }
      );

      sectionsRef.value.forEach((section) => {
        if (section) observer.observe(section);
      });
    };

    // Lifecycle hooks
    onMounted(() => {
      window.addEventListener('scroll', handleScroll);
      window.addEventListener('resize', handleResize);
      document.addEventListener('keydown', handleKeydown);
      initIntersectionObserver();
      calculateSectionProgress();
    });

    onBeforeUnmount(() => {
      persistState();
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('resize', handleResize);
      document.removeEventListener('keydown', handleKeydown);
      if (observer) observer.disconnect();
    });

    return {
      // State
      sections,
      activeSection,
      sectionsRef,
      isCompact,
      tooltipIndex,
      tooltipStyle,
      timeSpent,
      sectionVisits,
      sectionProgress,
      quizAnswers,
      showLoadMore,

      // Methods
      scrollToSection,
      showTooltip,
      hideTooltip,
      formatTime,
      getDifficultyClass,
      markAsComplete,
      isCompleted
    };
  }
};
</script>

<style scoped>
/* Custom styles for animations and complex elements */
.section-number {
  @apply absolute top-6 left-6 text-6xl font-bold text-gray-100 select-none;
}

.progress-bar {
  @apply w-full h-1 bg-gray-200 rounded-full overflow-hidden;
}

.progress-fill {
  @apply h-full bg-blue-500 transition-all duration-300;
}

.scroll-tracker {
  @apply fixed right-6 top-1/2 -translate-y-1/2 bg-white/90
  p-4 rounded-2xl shadow-lg backdrop-blur-sm;
}

.toggle-compact-btn {
  @apply absolute -left-6 top-1/2 -translate-y-1/2 w-6 h-6
  bg-white rounded-full shadow-md flex items-center justify-center
  hover:scale-110 transition-transform duration-200;
}

.tracker-item {
  @apply relative flex items-center h-12 cursor-pointer
  hover:bg-gray-100 rounded-lg transition-colors duration-200;
}

.progress-indicator {
  @apply w-1 h-8 bg-gray-200 rounded-full mx-2;
}

.tracker-item.active .progress-indicator {
  @apply bg-blue-500;
}

.tracker-item.visited .progress-indicator {
  @apply bg-green-500;
}
</style>