<script setup>
import { ref } from 'vue'

const newTask = ref('')

const tasks = ref([])

const addTask = () => {
  tasks.value.unshift({
    id: tasks.value.length + 1,
    label: newTask.value,
    completed: false,
  })
  newTask.value = ''
}

const deleteTask = index => {
  tasks.value.splice(index, 1)
}

const completeTask = index => {
  tasks.value[index].completed = !tasks.value[index].completed
}
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
header {
  line-height: 1.5;
}

h1 {
  font-size: 1.8em;
  font-weight: bold;
  transition: all 0.5s ease-in-out;
}

@keyframes title {
  to {
    color: var(--green-c-text);
    font-size: 38px;
  }
}

h1:hover {
  font-size: 2.25em;
  color: var(--green-c-text);
}

.todo-form {
  width: 60vw;
  padding: 1.5em;
  margin-top: 10vh;
  color: #000;
  background-color: var(--green-c-text);
  border: transparent;
  border-radius: 0.417em;
}

.task-input {
  width: 60%;
  margin-bottom: 0.208em;
  padding: 1.3em;
  font-size: 0.7em;
  border: transparent;
  border-radius: 0.157em;
  box-shadow: 0px 2.5px #0005;
  background-color: #fffb;
  transition: all 0.25s ease-in-out;

  &:focus {
    outline: none;
    background-color: #fffd;
  }

  &:hover {
    background-color: #fffd;
  }
}

.task-item {
  font-size: 0.75em;
  display: flex;
  align-items: center;
  font-weight: 600;
  margin-left: 0.208em;
  margin-top: 0.267em;
}

.btn {
  margin-left: 0.208em;
  border: transparent;

  transition: all 0.25s ease-in-out;
  &:hover {
    cursor: pointer;
    background-color: #bbb;
  }
}

.submit-btn {
  padding: 1.3em;
  font-size: 0.7em;
  border-radius: 0.157em;

  &:disabled {
    background-color: #0002;
  }

  &:disabled:hover {
    cursor: auto;
    background-color: #0002;
  }
}

.delete-btn {
  background-color: #f0f0f0;
  border-radius: 50%;
  padding: 0.315em;
  margin-right: 0.25em;
  /* padding-left: 0.208em; */
  &::before {
    content: '❌';
  }
  &:hover {
    border-radius: 50%;
    background-color: #bbb;
  }
}

.check-task {
  border-radius: 50%;
  margin-right: 0.208em;
  padding: 0.315em;

  &::before {
    content: '✔️';
    opacity: 0.25;

    transition: opacity 0.5s ease-in-out;
  }
}

.checked {
  padding: 0.315em;
  &::before {
    opacity: 1;
    filter: hue-rotate(230deg);
    transition: opacity 0.5s ease-in-out;
  }
}

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
