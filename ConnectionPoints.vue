<!-- ConnectionPoints.vue -->
<template>
  <template v-for="point in connectionPoints" :key="point.position">
    <div
        :class="[
        'absolute w-3.5 h-3.5 rounded-full transition-all duration-200',
        'hover:scale-125 hover:shadow-md',
        isConnecting ? 'bg-green-500' : 'bg-blue-500',
        `connection-point-${point.position}`,
        activePoint === point.position ? 'ring-2 ring-green-300 ring-opacity-50' : ''
      ]"
        :style="getPointStyle(point.position)"
        @mousedown.stop="startConnection($event, point.position)"
        @mousemove.stop="updateConnection($event, point.position)"
        @mouseup.stop="finishConnection($event, point.position)"
        @mouseenter="handleHover(point.position)"
        @mouseleave="handleLeave(point.position)"
    >
      <div
          v-if="activePoint === point.position && isConnecting"
          class="absolute inset-0 animate-ping rounded-full bg-blue-400 opacity-75"
      />
    </div>
  </template>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  node: {
    type: Object,
    required: true
  }
});

const emit = defineEmits([
  'start-connection',
  'update-connection',
  'finish-connection',
  'connection-hover',
  'connection-leave'
]);

const isConnecting = ref(false);
const activePoint = ref(null);

const connectionPoints = computed(() => [
  { position: 'left' },
  { position: 'right' },
  { position: 'top' },
  { position: 'bottom' }
]);

const getPointStyle = (position) => {
  const styles = {
    left: {
      left: '-6px',
      top: '50%',
      transform: 'translateY(-50%)',
      zIndex: 10
    },
    right: {
      right: '-6px',
      top: '50%',
      transform: 'translateY(-50%)',
      zIndex: 10
    },
    top: {
      top: '-6px',
      left: '50%',
      transform: 'translateX(-50%)',
      zIndex: 10
    },
    bottom: {
      bottom: '-6px',
      left: '50%',
      transform: 'translateX(-50%)',
      zIndex: 10
    }
  };
  return styles[position];
};

const startConnection = (event, position) => {
  event.stopPropagation();
  isConnecting.value = true;
  activePoint.value = position;

  const rect = event.target.getBoundingClientRect();
  const point = {
    x: rect.left + rect.width / 2,
    y: rect.top + rect.height / 2,
    position,
    nodeId: props.node.id
  };

  emit('start-connection', point);
};

const updateConnection = (event, position) => {
  if (!isConnecting.value) return;

  const rect = event.target.getBoundingClientRect();
  const point = {
    x: rect.left + rect.width / 2,
    y: rect.top + rect.height / 2,
    position,
    nodeId: props.node.id
  };

  emit('update-connection', point);
};

const finishConnection = (event, position) => {
  if (!isConnecting.value) return;

  const rect = event.target.getBoundingClientRect();
  const point = {
    x: rect.left + rect.width / 2,
    y: rect.top + rect.height / 2,
    position,
    nodeId: props.node.id
  };

  isConnecting.value = false;
  activePoint.value = null;
  emit('finish-connection', point);
};

const handleHover = (position) => {
  emit('connection-hover', props.node.id, position);
};

const handleLeave = (position) => {
  emit('connection-leave', props.node.id, position);
};
</script>

<style scoped>
.connection-point-left:hover,
.connection-point-right:hover,
.connection-point-top:hover,
.connection-point-bottom:hover {
  @apply scale-125 shadow-md;
}

@keyframes ping {
  75%, 100% {
    transform: scale(2);
    opacity: 0;
  }
}

.animate-ping {
  animation: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
}
</style>