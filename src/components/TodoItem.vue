<template>
  <div>
    <table style="width: 100%;">
      <tr>
        <td style="width: 8%;">
          <input type="checkbox" v-model="task.completed" @change="markAsComplete" class="ml-2" />
        </td>
        <td class= "bg-blue-200 rounded-lg p-1 ps-4">
          <label :class="{ 'line-through': task.completed }">
            {{ task.description }}
          </label>
        </td>
        <td style="width: 20%; text-align: right;">
          <Dialog>
            <DialogTrigger>
              <button class="px-2 py-1 bg-gray-400 text-white rounded-md ml-2">Edit</button>
            </DialogTrigger>
            <DialogContent>
              <DialogHeader>
                <DialogTitle>Edit Task</DialogTitle>
                <DialogDescription>
                  Edit the task description below. Click Save when you're done.
                </DialogDescription>
              </DialogHeader>
              <input v-model="editedDescription" class="w-full px-3 py-2 border rounded-md" />
              <DialogFooter>
                <DialogClose>
                  <button @click="saveChanges" class="px-4 py-2 bg-blue-500 text-white rounded-md mr-2">Save</button>
                  <button @click="closeDialog" class="px-4 py-2 bg-gray-300 text-gray-700 rounded-md">Cancel</button>
                </DialogClose>               
              </DialogFooter>
            </DialogContent>
          </Dialog>
          <button @click="deleteTask" class="px-2 py-1 bg-red-500 text-white rounded-md ml-2">Delete</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
import { defineProps, refs } from 'vue';
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
  DialogClose,
} from '@/components/ui/dialog';

const props = defineProps<{
  task: { description: string; completed: boolean };
  index: number;
  deleteTask: () => void;
}>(); 

let editedDescription = props.task.description;

const markAsComplete = () => {
  // This is handled in TodoList.vue
};

const saveChanges = () => {
  props.task.description = editedDescription;
  closeDialog();
};

const closeDialog = () => {
  editedDescription = props.task.description; // Reset the edited description
  $refs.dialog.close(); // Close the dialog
};
</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>
