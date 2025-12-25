<script setup>
import { ref, computed } from 'vue'
import TodoItem from './components/TodoItem.vue'

const newTodo = ref('')
const todos = ref([])
const filter = ref('all')

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      completed: false
    })
    newTodo.value = ''
  }
}

const deleteTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

const toggleTodo = (id) => {
  const todo = todos.value.find(todo => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const filteredTodos = computed(() => {
  if (filter.value === 'active') {
    return todos.value.filter(todo => !todo.completed)
  } else if (filter.value === 'completed') {
    return todos.value.filter(todo => todo.completed)
  }
  return todos.value
})

const remainingCount = computed(() => {
  return todos.value.filter(todo => !todo.completed).length
})
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-8 px-4">
    <div class="max-w-2xl mx-auto">
      <h1 class="text-4xl font-bold text-center text-gray-800 mb-8">Todo List</h1>
      
      <div class="bg-white rounded-lg shadow-lg p-6 mb-6">
        <form @submit.prevent="addTodo" class="flex gap-2">
          <input
            v-model="newTodo"
            type="text"
            placeholder="新しいタスクを追加..."
            class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
          <button
            type="submit"
            class="px-6 py-2 bg-blue-500 text-white font-medium rounded-lg hover:bg-blue-600 transition-colors"
          >
            追加
          </button>
        </form>
      </div>

      <div class="bg-white rounded-lg shadow-lg p-6">
        <div class="flex gap-2 mb-4">
          <button
            @click="filter = 'all'"
            :class="[
              'px-4 py-2 rounded-lg font-medium transition-colors',
              filter === 'all' 
                ? 'bg-blue-500 text-white' 
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            ]"
          >
            すべて
          </button>
          <button
            @click="filter = 'active'"
            :class="[
              'px-4 py-2 rounded-lg font-medium transition-colors',
              filter === 'active' 
                ? 'bg-blue-500 text-white' 
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            ]"
          >
            未完了
          </button>
          <button
            @click="filter = 'completed'"
            :class="[
              'px-4 py-2 rounded-lg font-medium transition-colors',
              filter === 'completed' 
                ? 'bg-blue-500 text-white' 
                : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            ]"
          >
            完了
          </button>
        </div>

        <div v-if="filteredTodos.length === 0" class="text-center py-8 text-gray-500">
          タスクがありません
        </div>

        <div v-else class="space-y-2">
          <TodoItem
            v-for="todo in filteredTodos"
            :key="todo.id"
            :todo="todo"
            @toggle="toggleTodo"
            @delete="deleteTodo"
          />
        </div>

        <div v-if="todos.length > 0" class="mt-4 pt-4 border-t border-gray-200 text-sm text-gray-600">
          残り {{ remainingCount }} 件
        </div>
      </div>
    </div>
  </div>
</template>
