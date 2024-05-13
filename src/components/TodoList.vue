<template>
<div class="w-1/2 mt-3 bg-gray-100 rounded-lg p-6">
    <h1 class="text-3xl font-bold mb-4">Todo List</h1>
    <div class="flex w-full"> 
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Add new task..." class="flex-1 ps-5 p-2 border border-gray-300 rounded-l-md mb-4 mr-0.5" /> <!-- Set flex to 1 to take remaining space -->
      <button @click="addTask" class="px-4 py-4 bg-blue-500 text-white rounded-r-md mb-4">Add Task</button>
    </div>
   
    <div class="flex flex-wrap mt-5">
      <div v-for="(task, index) in tasks" :key="index" class="w-full mb-2">
        <TodoItem :task="task" :deleteTask="() => deleteTask(index)" />
      </div>
    </div>
  </div>


</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoItem from './TodoItem.vue';

const localStorageKey: string = 'todoList';

const tasks = ref<{ description: string; completed: boolean }[]>([]);
const newTask = ref<string>('');

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ description: newTask.value, completed: false });
    saveTasks();
    newTask.value = '';
  }
};

const deleteTask = (index: number) => {
  tasks.value.splice(index, 1);
  saveTasks();
};

const saveTasks = () => {
  localStorage.setItem(localStorageKey, JSON.stringify(tasks.value));
};

onMounted(() => {
  const storedTasks = localStorage.getItem(localStorageKey);
  if (storedTasks) {
    tasks.value = JSON.parse(storedTasks);
  }
});
</script>
