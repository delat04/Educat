<template>
  <div
      :class="[
      'relative group rounded-lg',
      {'hover:bg-gray-800/30 transition-colors duration-200': !isExpanded && isDark,
       'hover:bg-gray-100 transition-colors duration-200': !isExpanded && !isDark}
    ]"
      @contextmenu.prevent="showContextMenu"
  >
    <div
        :class="[
        'flex items-center px-3 py-1.5 cursor-pointer rounded-lg',
        { 'pl-[calc(theme(spacing[4])*level)]': level > 0 }
      ]"
        :style="{ paddingLeft: `calc(1rem * ${level})` }"
        @click="toggleFolder"
    >
      <!-- Folder/File Icon -->
      <component
          :is="icon"
          :class="[
    'w-4 h-4 mr-2.5 flex-shrink-0',
    item.type === 'folder' ? (isDark ? 'text-blue-400' : 'text-black') : isDark ? 'text-gray-400' : 'text-gray-600'
  ]"
          style="fill:rgb(238 185 106 / 0%)"
      />

      <!-- Name -->
      <span
          :class="[
          'text-sm truncate',
          isDark ? 'text-gray-300' : 'text-gray-900'
        ]"
      >{{ item.name }}</span>

      <!-- Modified Time -->
      <span
          v-if="item.modified"
          :class="[
          'ml-auto text-xs opacity-0 group-hover:opacity-100 transition-opacity duration-200',
          isDark ? 'text-gray-500' : 'text-gray-600'
        ]"
      >
        {{ item.modified }}
      </span>
    </div>

    <!-- Context Menu -->
    <div
        v-if="contextMenuVisible"
        :style="{ top: contextMenuY + 'px', left: contextMenuX + 'px' }"
        :class="[
        'fixed z-50 min-w-[180px] py-1.5 rounded-lg shadow-lg',
        isDark ? 'bg-gray-800 border-gray-700' : 'bg-white border-gray-200'
      ]"
        class="border"
        @click.stop
    >
      <button
          v-if="item.type === 'folder'"
          :class="[
          'w-full px-4 py-2 text-sm text-left flex items-center gap-2.5',
          isDark ? 'text-gray-300 hover:bg-gray-700' : 'text-gray-700 hover:bg-gray-100'
        ]"
          @click="addItem('file')"
      >
        <FileText class="w-4 h-4" :class="isDark ? 'text-gray-400' : 'text-gray-600'" />
        New File
      </button>
      <button
          v-if="item.type === 'folder'"
          :class="[
          'w-full px-4 py-2 text-sm text-left flex items-center gap-2.5',
          isDark ? 'text-gray-300 hover:bg-gray-700' : 'text-gray-700 hover:bg-gray-100'
        ]"
          @click="addItem('folder')"
      >
        <Folder class="w-4 h-4 text-blue-400" />
        New Folder
      </button>
      <div
          v-if="item.type === 'folder'"
          :class="isDark ? 'bg-gray-700' : 'bg-gray-200'"
          class="h-px my-1.5"
      ></div>
      <button
          class="w-full px-4 py-2 text-sm text-red-600 text-left flex items-center gap-2.5"
          :class="isDark ? 'hover:bg-gray-700' : 'hover:bg-gray-100'"
          @click="deleteItem"
      >
        <Trash2 class="w-4 h-4" />
        Delete
      </button>
    </div>

    <!-- Children -->
    <TransitionGroup
        v-if="item.type === 'folder'"
        name="children"
        class="space-y-0.5 mt-0.5"
    >
      <FileItem
          v-show="isExpanded"
          v-for="(child, index) in item.children"
          :key="child.name"
          :item="child"
          :level="level + 1"
          :is-last="index === item.children.length - 1"
          :is-dark="isDark"
          @click="$emit('click', child)"
          @update:items="$emit('update:items')"
      />
    </TransitionGroup>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import {
  Folder, FolderOpen, FileText,
  FileJson, FileCode, FileImage,
  Trash2
} from 'lucide-vue-next'

const props = defineProps({
  item: {
    type: Object,
    required: true
  },
  level: {
    type: Number,
    default: 0
  },
  isLast: {
    type: Boolean,
    default: false
  },
  isDark: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits(['click', 'update:items'])

const isExpanded = ref(true)
const contextMenuVisible = ref(false)
const contextMenuX = ref(0)
const contextMenuY = ref(0)

const icon = computed(() => {
  if (props.item.type === 'folder') {
    return isExpanded.value ? FolderOpen : Folder
  }

  const extension = props.item.name.split('.').pop()?.toLowerCase()

  switch(extension) {
    case 'json':
      return FileJson
    case 'js':
    case 'vue':
    case 'ts':
      return FileCode
    case 'png':
    case 'jpg':
    case 'jpeg':
    case 'gif':
    case 'ico':
      return FileImage
    default:
      return FileText
  }
})

const showContextMenu = (event) => {
  event.preventDefault()
  const maxX = window.innerWidth - 180
  const maxY = window.innerHeight - 150

  contextMenuX.value = Math.min(event.clientX, maxX)
  contextMenuY.value = Math.min(event.clientY, maxY)
  contextMenuVisible.value = true
}

const hideContextMenu = () => {
  contextMenuVisible.value = false
}

const addItem = (type) => {
  const itemName = prompt(`Enter ${type} name:`)
  if (itemName) {
    if (!props.item.children) {
      props.item.children = []
    }

    props.item.children.push({
      name: itemName,
      type: type,
      modified: 'just now',
      children: type === 'folder' ? [] : undefined
    })
    emit('update:items')
  }
  hideContextMenu()
}

const deleteItem = () => {
  if (confirm(`Are you sure you want to delete ${props.item.name}?`)) {
    const parent = findParent(props.item)
    if (parent && parent.children) {
      const index = parent.children.findIndex(child => child.name === props.item.name)
      if (index !== -1) {
        parent.children.splice(index, 1)
        emit('update:items')
      }
    }
  }
  hideContextMenu()
}

const toggleFolder = () => {
  if (props.item.type === 'folder') {
    isExpanded.value = !isExpanded.value
  } else {
    emit('click', props.item)
  }
}

const handleClickOutside = (event) => {
  if (contextMenuVisible.value) {
    hideContextMenu()
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
  document.addEventListener('contextmenu', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
  document.removeEventListener('contextmenu', handleClickOutside)
})
</script>

<style scoped>
.children-enter-active,
.children-leave-active {
  transition: all 0.3s ease;
}

.children-enter-from,
.children-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>