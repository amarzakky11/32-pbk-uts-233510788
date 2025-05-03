<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filtered = ref('all')

const addTask = () => {
  if (newTask.value !== '') {
    tasks.value.push({
      id: tasks.value.length + 1,
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const toggleTask = (task) => {
  task.completed == !task.completed
  console.log(task.completed)
}

const filteredTasks = computed(() => {
  if (filtered.value === 'all') {
    return tasks.value
  } else if (filtered.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else {
    return tasks.value.filter(task => !task.completed)
  }
})

</script>

<template>
  <div class="min-h-screen w-screen bg-gray-900 text-white flex items-center justify-center p-6">
    <div class="bg-gray-800 rounded-2xl shadow-xl w-full max-w-5xl h-[90vh] grid grid-cols-2 gap-4 overflow-hidden p-6">
      
      <!-- Form Input -->
      <div class="flex flex-col justify-between">
        <div>
          <h1 class="text-3xl font-bold mb-4">To-Do Manager</h1>
          <input
            type="text"
            v-model="newTask"
            @keyup.enter="addTask"
            placeholder="Add a new task"
            class="w-full p-3 rounded-md bg-gray-700 placeholder-gray-400 text-white mb-4 focus:outline-none focus:ring-2 focus:ring-purple-500 transition"
          />
          <button
            @click="addTask"
            class="w-full bg-purple-600 hover:bg-purple-700 transition p-3 rounded-md font-semibold shadow-lg"
          >
            Add Task
          </button>
        </div>

        <!-- Filter -->
        <div class="mt-8">
          <label class="block mb-2 text-sm font-semibold text-gray-300">Filter:</label>
          <select
            v-model="filtered"
            class="w-full p-2 bg-gray-700 text-white rounded-md shadow focus:outline-none focus:ring-2 focus:ring-purple-500"
          >
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="incomplete">Incomplete</option>
          </select>
        </div>
      </div>

      <!-- Task List -->
      <div class="bg-gray-700 rounded-xl p-4 overflow-y-auto custom-scroll">
        <ul class="space-y-3">
          <li
            v-for="task in filteredTasks"
            :key="task.id"
            class="bg-gray-800 p-3 rounded-lg flex items-center justify-between shadow-md transition-transform hover:scale-[1.02]"
          >
            <div class="flex items-center gap-3">
              <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" class="accent-purple-500 w-5 h-5" />
              <span :class="{'line-through text-gray-400': task.completed}" class="transition-all">{{ task.text }}</span>
            </div>
            <button @click="removeTask(task)" class="text-red-400 hover:text-red-600 transition">✕</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Cegah window scroll */
html,
body {
  overflow: hidden;
}

/* Custom scroll untuk kontainer tugas */
.custom-scroll::-webkit-scrollbar {
  width: 6px;
}
.custom-scroll::-webkit-scrollbar-thumb {
  background-color: rgba(156, 163, 175, 0.4); /* gray-400 */
  border-radius: 3px;
}
.custom-scroll {
  scrollbar-width: thin;
  scrollbar-color: rgba(156, 163, 175, 0.4) transparent;
}
</style>

