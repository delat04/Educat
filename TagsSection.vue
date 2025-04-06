<!-- TagsSection.vue -->
<template>
  <div class="flex flex-wrap gap-1.5">
    <span
        v-for="tag in tags"
        :key="tag"
        class="px-2.5 py-1 text-xs rounded-full bg-blue-50 text-blue-600 font-medium transition-colors duration-200 hover:bg-blue-100 border border-blue-200"
    >
      {{ tag }}
      <button
          v-if="isEditing"
          @click="removeTag(tag)"
          class="ml-1.5 text-blue-400 hover:text-blue-600"
      >
        ×
      </button>
    </span>

    <div v-if="isEditing" class="flex gap-1.5">
      <input
          v-model="newTag"
          @keyup.enter="addTag"
          placeholder="Add tag"
          class="px-2.5 py-1 text-xs rounded-full border border-slate-200 focus:border-blue-400 focus:ring-1 focus:ring-blue-400 min-w-[80px]"
      />
      <button
          @click="addTag"
          class="px-2.5 py-1 text-xs rounded-full bg-blue-500 text-white font-medium hover:bg-blue-600"
      >
        Add
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  tags: {
    type: Array,
    default: () => []
  },
  isEditing: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['update:tags']);
const newTag = ref('');

const addTag = () => {
  if (newTag.value.trim()) {
    const updatedTags = [...props.tags, newTag.value.trim()];
    emit('update:tags', updatedTags);
    newTag.value = '';
  }
};

const removeTag = (tagToRemove) => {
  const updatedTags = props.tags.filter(tag => tag !== tagToRemove);
  emit('update:tags', updatedTags);
};
</script>

