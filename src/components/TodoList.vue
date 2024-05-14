<template>
  <div class="w-3/4 mt-3 bg-gray-100 rounded-lg p-6">
    <h1 class="text-3xl font-bold mb-4">Todo List</h1>
    <div class="flex w-full"> 
      <input v-model="newTaskTitle" @keyup.enter="addTask" placeholder="Title" class="w-1/3 ps-5 p-2 border border-gray-300 rounded-l-md mb-4 mr-0.5" /> 
      <input v-model="newTaskDescription" @keyup.enter="addTask" placeholder="Description" class="w-2/3 ps-5 p-2 border border-gray-300 rounded-r-md mb-4 mr-0.5" />
      <button @click="addTask" class="px-4 py-4 bg-blue-500 text-white rounded-r-md mb-4">Add</button>
    </div>
   
    <div class="flex flex-wrap mt-5">
      <div class="w-full mb-2">
        <TodoItem
          v-for="(task, index) in tasks"
          :key="index"
          :task="task"
          :index="index"
          :deleteTask="deleteTask"
          :updateTask="updateTask"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoItem from './TodoItem.vue';
import { useToast } from 'vue-toastification'; 

const localStorageKey: string = 'todoList';

const tasks = ref<{ title: string; description: string; completed: boolean }[]>([]);
const newTaskTitle = ref<string>('');
const newTaskDescription = ref<string>('');

const addTask = () => {
  const toast = useToast(); 
  let errorMessage = '';

  if (newTaskTitle.value.trim() === '') {
    errorMessage = 'Task title';
  }
  if (newTaskDescription.value.trim() === '') {
    if (errorMessage !== '') {
      errorMessage += ' and ';
    }
    errorMessage += 'Task description';
  }

  if (errorMessage !== '') {
    errorMessage += ' cannot be empty';
    toast.error(errorMessage, {
      timeout: 2000,
    });
  } else {
    tasks.value.push({ title: newTaskTitle.value, description: newTaskDescription.value, completed: false });
    saveTasks();
    newTaskTitle.value = '';
    newTaskDescription.value = '';
  }
};

const deleteTask = (index: number) => {
  tasks.value.splice(index, 1);
  saveTasks();
};

const updateTask = (index: number, updatedTask: { title: string; description: string; completed: boolean }) => {
  tasks.value[index] = updatedTask;
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
