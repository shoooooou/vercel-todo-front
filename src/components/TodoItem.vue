<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <div class="flex items-center gap-3 p-3 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors">
    <input
      type="checkbox"
      :checked="todo.completed"
      @change="emit('toggle', todo.id)"
      class="w-5 h-5 text-blue-500 rounded cursor-pointer"
    />
    <span
      :class="[
        'flex-1 text-gray-800',
        todo.completed && 'line-through text-gray-400'
      ]"
    >
      {{ todo.text }}
    </span>
    <button
      @click="emit('delete', todo.id)"
      class="px-3 py-1 text-sm text-red-600 hover:bg-red-100 rounded transition-colors"
    >
      削除
    </button>
  </div>
</template>
