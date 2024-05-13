<template>
  <div class="w-3/4 mt-3 bg-gray-100 rounded-lg p-6">
    <Alert v-if="showAlert" variant="destructive" class="mb-2">
      <AlertTitle>Error</AlertTitle>
      <AlertDescription>
        Title is a required parameter!
      </AlertDescription>
    </Alert>
    <h1 class="text-3xl font-bold mb-4">Todo List</h1>
    <div class="flex w-full"> 
      <input v-model="newTaskTitle" @keyup.enter="addTask" placeholder="Title" class="w-1/3 ps-5 p-2 border border-gray-300 rounded-l-md mb-4 mr-0.5" /> 
      <input v-model="newTaskDescription" @keyup.enter="addTask" placeholder="Description" class="w-2/3 ps-5 p-2 border border-gray-300 rounded-r-md mb-4 mr-0.5" />
      <button @click="addTask" class="px-4 py-4 bg-blue-500 text-white rounded-r-md mb-4">Add</button>
    </div>
   
    <div class="flex flex-wrap mt-5">
      <div v-for="(task, index) in tasks" :key="index" class="w-full mb-2">
        <TodoItem :task="task" :deleteTask="() => deleteTask(index)"/>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoItem from './TodoItem.vue';
import { Alert, AlertDescription, AlertTitle } from '@/components/ui/alert'

const localStorageKey: string = 'todoList';

const tasks = ref<{ title: string; description: string; completed: boolean }[]>([]);
const newTaskTitle = ref<string>('');
const newTaskDescription = ref<string>('');
const showAlert = ref<boolean>(false);

const addTask = () => {
  if (newTaskTitle.value.trim() !== '') {
    tasks.value.push({ title: newTaskTitle.value, description: newTaskDescription.value, completed: false });
    saveTasks();
    newTaskTitle.value = '';
    newTaskDescription.value = '';
    showAlert.value = false;
  } else {
    showAlert.value = true;
  }
};

const deleteTask = (index: number) => {
  tasks.value.splice(index, 1);
  showAlert.value = false;
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
