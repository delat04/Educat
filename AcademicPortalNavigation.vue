<!-- AcademicNav.vue -->
<template>
  <div class="nav-container w-72 min-h-[calc(100vh-3rem)] relative">
    <!-- Header Section -->
    <div class="menu-header p-6 mb-4">
      <h1 class="text-xl font-semibold text-slate-800 mb-2">Copedia</h1>
      <p class="text-sm text-slate-500">Educational Platform Verse</p>

      <!-- Search Box -->
      <div class="search-box mt-4">
        <i class="fas fa-search search-icon"></i>
        <input
            v-model="searchTerm"
            type="text"
            placeholder="Search..."
            class="w-full pl-10 pr-4 py-2 rounded-lg border border-slate-200 focus:outline-none focus:border-blue-400 focus:ring-2 focus:ring-blue-100 text-sm transition-all duration-300"
        >
      </div>
    </div>

    <!-- Navigation Menu -->
    <nav class="px-4">
      <!-- Menu Sections -->
      <div v-for="(section, index) in filteredMenuSections" :key="index" class="mb-6">
        <div
            class="menu-item rounded-lg p-3 cursor-pointer group"
            :class="{ active: section.isOpen }"
            @click="toggleSection(section)"
            :data-tooltip="section.tooltip"
        >
          <div class="highlight-bar rounded-l-lg"></div>
          <div class="flex items-center">
            <i :class="[section.icon, section.iconColor, 'w-5']"></i>
            <span class="ml-3 font-medium text-slate-700">{{ section.title }}</span>
            <i
                class="fas fa-chevron-down ml-auto text-slate-400 menu-icon"
                :class="{ 'rotate-icon': section.isOpen }"
            ></i>
          </div>
        </div>

        <!-- Submenu -->
        <div class="submenu pl-8 mt-2" :class="{ active: section.isOpen }">
          <div
              v-for="(item, itemIndex) in section.items"
              :key="itemIndex"
              class="menu-item py-2 px-3 text-sm text-slate-600 rounded-md"
          >
            <i :class="[item.icon, 'mr-2 text-slate-400']"></i>
            {{ item.label }}
            <span
                v-if="item.badge"
                :class="[
                'ml-2 text-xs px-2 py-1 rounded-full',
                `bg-${item.badge.color}-100`,
                `text-${item.badge.color}-600`
              ]"
            >
              {{ item.badge.text }}
            </span>
          </div>
        </div>
      </div>
    </nav>

    <!-- Profile Section -->
    <div class="profile-section absolute bottom-0 left-0 w-full p-6 bg-white border-t border-slate-200">
      <div class="flex items-center space-x-3">
        <div class="avatar w-10 h-10 rounded-full flex items-center justify-center text-white shadow-lg cursor-pointer">
          <i class="fas fa-user"></i>
        </div>
        <div class="flex-1">
          <div class="font-medium text-slate-800">{{ user.name }}</div>
          <div class="text-sm text-slate-500">{{ user.role }}</div>
        </div>
        <div class="text-slate-400 hover:text-slate-600 cursor-pointer">
          <i class="fas fa-cog"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// User data
const user = ref({
  name: 'John Doe',
  role: 'Student'
})

// Search functionality
const searchTerm = ref('')

// Menu sections data
const menuSections = ref([
  {
    title: 'Progress',
    icon: 'fas fa-chart-line',
    iconColor: 'text-blue-600',
    tooltip: 'Track your academic progress',
    isOpen: false,
    items: [
      { label: 'Dashboard', icon: 'fas fa-tachometer-alt' },
      { label: 'Enrolled Courses', icon: 'fas fa-graduation-cap' },
      { label: 'Income', icon: 'fas fa-coins' }
    ]
  },
  {
    title: 'Courses',
    icon: 'fas fa-book',
    iconColor: 'text-emerald-600',
    tooltip: 'Browse available courses',
    isOpen: false,
    items: [
      {
        label: 'Mathematics',
        icon: 'fas fa-square-root-alt',
        badge: { text: 'New', color: 'blue' }
      },
      { label: 'Physics', icon: 'fas fa-atom' },
      { label: 'Chemistry', icon: 'fas fa-flask' }
    ]
  },
  {
    title: 'Meeting',
    icon: 'fas fa-users',
    iconColor: 'text-purple-600',
    tooltip: 'Schedule and join meetings',
    isOpen: false,
    items: [
      {
        label: 'Meetings',
        icon: 'fas fa-video',
        badge: { text: 'Live', color: 'red' }
      },
      { label: 'Registration', icon: 'fas fa-calendar-plus' }
    ]
  }
])

// Computed property for filtered menu sections
const filteredMenuSections = computed(() => {
  if (!searchTerm.value) return menuSections.value

  return menuSections.value.filter(section =>
      section.title.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
      section.items.some(item =>
          item.label.toLowerCase().includes(searchTerm.value.toLowerCase())
      )
  )
})

// Toggle section
const toggleSection = (section) => {
  section.isOpen = !section.isOpen
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap');

.nav-container {
  background: linear-gradient(to bottom, #ffffff, #f8fafc);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  border-radius: 24px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.nav-container:hover {
  box-shadow: 0 6px 25px rgba(0, 0, 0, 0.12);
}

.menu-item {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.menu-item:hover {
  transform: translateX(8px);
  background: rgba(59, 130, 246, 0.05);
}

.menu-item.active {
  background: rgba(59, 130, 246, 0.1);
}

.submenu {
  max-height: 0;
  overflow: hidden;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  opacity: 0;
  transform: translateY(-10px);
}

.submenu.active {
  max-height: 500px;
  opacity: 1;
  transform: translateY(0);
}

.menu-icon {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.rotate-icon {
  transform: rotate(180deg);
}

.highlight-bar {
  position: absolute;
  left: 0;
  top: 0;
  width: 4px;
  height: 100%;
  background: linear-gradient(to bottom, #3b82f6, #8b5cf6);
  opacity: 0;
  transition: all 0.3s ease;
}

.menu-item:hover .highlight-bar {
  opacity: 1;
}

.avatar {
  background: linear-gradient(135deg, #3b82f6, #8b5cf6);
  transition: transform 0.3s ease;
}

.avatar:hover {
  transform: scale(1.1);
}

.search-box {
  position: relative;
  transition: all 0.3s ease;
}

.search-box:focus-within {
  transform: translateY(-2px);
}

.search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #94a3b8;
}
</style>