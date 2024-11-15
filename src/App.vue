<script setup>
import { ref, onMounted } from 'vue'

const newTask = ref('')

const tasks = ref([])

const loadTasks = () => {
  const storedTasks = localStorage.getItem('tasks')
  if (storedTasks) tasks.value = JSON.parse(storedTasks)
}

const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

const addTask = () => {
  tasks.value.unshift({
    id: tasks.value.length - 1,
    label: newTask.value,
    completed: false,
  })
  newTask.value = ''
  saveTasks()
}

const deleteTask = index => {
  tasks.value.splice(index, 1)
  saveTasks()
}

const completeTask = index => {
  tasks.value[index].completed = !tasks.value[index].completed
  saveTasks()
}

onMounted(loadTasks)
</script>

<template>
  <header>
    <div class="wrapper">
      <h1>TODO List</h1>
    </div>
  </header>
  <main>
    <form class="todo-form" @submit.prevent="addTask">
      <input
        class="task-input"
        v-model.trim="newTask"
        type="text"
        placeholder="Add a task"
      />
      <button
        type="submit"
        class="btn submit-btn"
        :disabled="newTask.length < 5"
      >
        Add Task
      </button>
      <div class="tasks-container">
        <transition-group name="fade" tag="div">
          <p
            class="task-item"
            v-for="(task, index) in tasks"
            v-bind:key="task.id"
          >
            <button
              type="button"
              v-if="tasks.length > 0"
              class="btn check-task"
              :class="{ checked: task.completed }"
              @click="completeTask(index)"
            ></button>
            <button
              @click="deleteTask(index)"
              type="button"
              class="btn delete-btn"
            ></button>
            {{ task.label }}
          </p>
        </transition-group>
      </div>
    </form>
  </main>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
