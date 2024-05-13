<template>
  <div>
    <table style="width: 100%;">
      <tr>
        <td style="width: 8%;">
          <input type="checkbox" v-model="task.completed" @change="markAsComplete" class="ml-2" style="transform: scale(1.5);" />
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
              <button @click="openDialog" class="px-3 py-1 bg-gray-400 text-white rounded-md ml-2 py-3">Edit</button>
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
              <Alert v-if="showAlert" variant="destructive" class="mb-1">
                  <AlertTitle>Error</AlertTitle>
                  <AlertDescription>
                    Title is a required parameter!
                  </AlertDescription>
                </Alert>
              <DialogFooter>               
                <button @click="saveChanges" class="px-4 py-2 bg-blue-500 text-white rounded-md mr-2">Save</button>
                <button @click="closeDialog" class="px-4 py-2 bg-gray-300 text-gray-700 rounded-md">Cancel</button>           
              </DialogFooter>
            </DialogContent>
          </Dialog>
          <button @click="deleteTask" class="px-2 py-1 bg-red-500 text-white rounded-md ml-2 py-3">Delete</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Alert, AlertDescription, AlertTitle } from '@/components/ui/alert';
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
  deleteTask: () => void;
}>(); 

let dialogOpen = ref(false);
let editedTitle = props.task.title;
let editedDescription = props.task.description;
let showAlert = ref(false);

const openDialog = () => {
  dialogOpen.value = true;
};

const closeDialog = () => {
  dialogOpen.value = false;
  editedTitle = ""; 
  editedDescription = ""; 
  showAlert.value = false; 
};

const markAsComplete = () => {
  // This is handled in TodoList.vue
};

const saveChanges = () => {
  if (editedTitle.trim() !== '') {
    props.task.title = editedTitle;
    props.task.description = editedDescription;
    closeDialog();
  } else {
    showAlert.value = true;
    openDialog();
  }
};

</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>
