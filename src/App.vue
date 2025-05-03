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
  <div>
    <input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task">
    <button @click="addTask">Add Task</button>
    <select v-model="filtered">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="incomplete">Incomplete</option>
    </select>

    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" @change="toggleTask(task)" v-model="task.completed">
        {{ task.text }}
        <button @click="removeTask(task)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
