<template>
  <div>
    <table style="width: 100%;" class="my-2">
      <tr>
        <td style="width: 8%;">
          <input type="checkbox" v-model="task.completed" @change="markAsComplete(index)" class="ml-3" style="transform: scale(1.5);" />
        </td>
        <td class="bg-blue-200 rounded-lg p-1 ps-4">
          <label :class="{ 'line-through': task.completed }">
            <strong>{{ task.title }}</strong><br>
            <small>{{ task.description }}</small>
          </label>
        </td>
        <td style="width: 15%; text-align: right;">
          <Dialog :open="dialogOpen">
            <DialogTrigger>
              <button @click="openDialog" class="px-3 bg-gray-400 text-white rounded-md ml-2 py-3">Edit</button>
            </DialogTrigger>
            <DialogContent>
              <DialogHeader>
                <DialogTitle>Edit Task</DialogTitle>
                <DialogDescription>
                  Edit the task details below. Click Save when you're done.
                </DialogDescription>
              </DialogHeader>
              <label for="editedTitle" class="block">Title:</label>
              <input id="editedTitle" v-model="editedTitle" class="w-full px-3 py-2 border rounded-md mb-2" placeholder="Title" />
              <label for="editedDescription" class="block">Description:</label>
              <input id="editedDescription" v-model="editedDescription" class="w-full px-3 py-2 border rounded-md mb-2" placeholder="Description" />             
              <DialogFooter>               
                <button @click="saveChanges" class="px-4 py-2 bg-blue-500 text-white rounded-md mr-2">Save</button>
                <button @click="closeDialog" class="px-4 py-2 bg-gray-300 text-gray-700 rounded-md">Cancel</button>           
              </DialogFooter>
            </DialogContent>
          </Dialog>
          <button @click="deleteTask(index)" class="px-2 bg-red-500 text-white rounded-md ml-2 py-3">Delete</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useToast } from 'vue-toastification'; 
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog';

const props = defineProps<{
  task: { title: string; description: string; completed: boolean };
  index: number;
  deleteTask: (index: number) => void;
  updateTask: (index: number, updatedTask: { title: string; description: string; completed: boolean }) => void;
}>();

let dialogOpen = ref(false);
let editedTitle = props.task.title;
let editedDescription = props.task.description;

const openDialog = () => {
  dialogOpen.value = true;
};

const closeDialog = () => {
  dialogOpen.value = false;
  editedTitle = ""; 
  editedDescription = ""; 
};

const markAsComplete = (index: number) => {
  props.updateTask(index, { ...props.task, completed: props.task.completed });
};

const saveChanges = () => {
  const toast = useToast(); 
  let errorMessage = '';

  if (editedTitle.trim() === '') {
    errorMessage = 'Task title';
  }
  if (editedDescription.trim() === '') {
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
    props.updateTask(props.index, {
      title: editedTitle,
      description: editedDescription,
      completed: props.task.completed,
    });
    closeDialog();
  }
};
</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>
