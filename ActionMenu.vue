<template>
  <div class="relative">
    <button
        @click="isOpen = !isOpen"
        class="p-1.5 rounded-full hover:bg-slate-50 opacity-0 group-hover:opacity-100 transition-all duration-200"
    >
      <MoreHorizontal class="w-4 h-4 text-slate-500 hover:text-slate-700" />
    </button>

    <div
        v-if="isOpen"
        class="absolute right-0 mt-1 w-48 bg-white rounded-lg shadow-lg z-10 py-1 border border-slate-100"
        @mouseleave="isOpen = false"
    >
      <button
          v-for="action in actions"
          :key="action.id"
          @click="handleAction(action.id)"
          class="w-full px-4 py-2.5 text-sm text-left hover:bg-slate-50 flex items-center gap-3"
      >
        <component :is="action.icon" class="w-4 h-4 text-slate-500" />
        <span class="text-slate-700">{{ action.label }}</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { Edit, Copy, Lock, Trash2 } from 'lucide-vue-next';

const emit = defineEmits(['edit', 'duplicate', 'delete', 'lock']);
const isOpen = ref(false);

const actions = [
  { id: 'edit', label: 'Edit', icon: Edit },
  { id: 'duplicate', label: 'Duplicate', icon: Copy },
  { id: 'lock', label: 'Lock Position', icon: Lock },
  { id: 'delete', label: 'Delete', icon: Trash2 }
];

const handleAction = (actionId) => {
  emit(actionId);
  isOpen.value = false;
};
</script>

