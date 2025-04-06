<!-- Navbar.vue -->
<template>
  <nav
      ref="navbar"
      :class="[
      'fixed top-0 w-full z-50 transition-all duration-500 ease-in-out',
      isVertical ? 'right-0 h-screen bg-gradient-to-b from-indigo-600 to-purple-600 shadow-2xl' : 'bg-white shadow-md'
    ]"
      :style="{ width: isVertical ? navbarHeight + 'px' : '100%' }"
  >
    <!-- Navbar Content -->
    <div
        :class="[
        'flex items-center justify-between px-6 py-4',
        isVertical ? 'flex-col h-full' : 'flex-row'
      ]"
    >
      <!-- Logo -->
      <div class="flex items-center">
        <span
            :class="[
            'text-xl font-bold transition-colors duration-300',
            isVertical ? 'text-white rotate-90 mt-12' : 'text-indigo-600'
          ]"
        >
          Productivity
        </span>
      </div>

      <!-- Navigation Links -->
      <ul
          :class="[
          'flex transition-all duration-300',
          isVertical ? 'flex-col space-y-6 mt-12 text-white text-center' : 'space-x-6'
        ]"
      >
        <li v-for="(item, index) in navItems" :key="index">
          <a
              href="#"
              @click.prevent="handleNavClick(item)"
              :class="[
              'relative text-sm font-medium transition-all duration-300 group',
              isVertical ? 'text-white hover:text-indigo-200' : 'text-gray-700 hover:text-indigo-600'
            ]"
          >
            {{ item }}
            <span
                :class="[
                'absolute bottom-0 left-0 w-full h-0.5 transform origin-left transition-transform duration-300',
                isVertical ? 'bg-white scale-x-0 group-hover:scale-x-100' : 'bg-indigo-600 scale-x-0 group-hover:scale-x-100'
              ]"
            ></span>
          </a>
        </li>
      </ul>

      <!-- Toggle Button -->
      <button
          @click="toggleVertical"
          class="focus:outline-none p-2 rounded-full hover:bg-indigo-100 transition-colors duration-300"
          :class="isVertical ? 'text-white hover:bg-white/20 mb-6' : 'text-indigo-600'"
      >
        <svg
            class="w-6 h-6 transform transition-transform duration-300"
            :class="{ 'rotate-90': isVertical }"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
        >
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
        </svg>
      </button>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
  data() {
    return {
      isVertical: false,
      lastScrollY: 0,
      navbarHeight: 0,
      navItems: ['Home', 'Projects', 'Tasks', 'Analytics']
    }
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll)
    this.updateNavbarHeight()
    window.addEventListener('resize', this.updateNavbarHeight)
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll)
    window.removeEventListener('resize', this.updateNavbarHeight)
  },
  methods: {
    handleScroll() {
      const currentScrollY = window.scrollY
      if (currentScrollY > this.lastScrollY && currentScrollY > 100) {
        // Scrolling down past 100px
        this.isVertical = true
      } else if (currentScrollY < 100) {
        // Scrolling up near top
        this.isVertical = false
      }
      this.lastScrollY = currentScrollY
    },
    toggleVertical() {
      this.isVertical = !this.isVertical
    },
    handleNavClick(item) {
      console.log(`Navigated to: ${item}`)
      this.$emit('navigate', item)
    },
    updateNavbarHeight() {
      const navbar = this.$refs.navbar
      if (navbar && !this.isVertical) {
        this.navbarHeight = navbar.offsetHeight
      }
    }
  },
  watch: {
    isVertical(newVal) {
      if (!newVal) {
        this.updateNavbarHeight() // Update height when switching back to horizontal
      }
    }
  }
}
</script>

<style scoped>
/* Smooth transitions for navbar */
nav {
  transform-origin: top right;
}

/* Slide-in animation for vertical mode */
@media (min-width: 768px) {
  nav.h-screen {
    transform: translateX(100%);
    animation: slideIn 0.5s forwards;
  }
  nav:not(.h-screen) {
    transform: translateX(0);
  }
}

@keyframes slideIn {
  0% { transform: translateX(100%); }
  100% { transform: translateX(0); }
}
</style>