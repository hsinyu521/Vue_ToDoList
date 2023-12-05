<script setup>
import { ref, onBeforeMount } from 'vue';

const newTask = ref('');
const tasks = ref([]);

const loadTasksFromStorage = () => {
  const storedTasks = localStorage.getItem('tasks');
  if (storedTasks) {
    tasks.value = JSON.parse(storedTasks);
  }
};

const saveTasksToStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

onBeforeMount(() => {
  loadTasksFromStorage();
});

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.unshift({ text: newTask.value.trim(), completed: false });
    newTask.value = '';
    saveTasksToStorage(); 
  }
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
  saveTasksToStorage(); 
};
</script>

<template>
  <head>
  <title>TODO APP</title>
  </head>
  <body>
    <h1>TO DO LIST</h1>
      <form @submit.prevent="addTask">
        <input type="text" v-model="newTask" />
        <button type="submit">新增</button>
      </form>
      <ul>
        <li v-for="(task, index) in tasks" :key="index" :class="{ 'completed': task.completed }">
          <input type="checkbox" v-model="task.completed" />
          <span :style="{ 'text-decoration': task.completed ? 'line-through' : 'none' }">{{ task.text }}</span>
          <button @click="removeTask(index)">X</button>
        </li>
      </ul>
  </body>
</template>

<style scoped>
.completed {
  color: #888;
}
h1 {
  font-size: 24px; 
  margin-bottom: 10px; 
  margin-left: 10px;
}

form {
  margin-bottom: 10px;
}

input[type="text"] {
  margin-bottom: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
button{
  margin-left: 3px;
}
</style>
