<script setup lang="ts">
import { ref } from "vue";

const newTask = ref("");
const error = ref("");
const emit = defineEmits<{
  addTask: [newTask: string];
}>();

function addTask() {
  if (newTask.value.trim() === "") {
    error.value = "Task cannot be empty!";
    return;
  }
  emit("addTask", newTask.value.trim());
  error.value = "";
  newTask.value = "";
}
</script>

<template>
  <form @submit.prevent="addTask">
    <div class="mb-4">
      <label for="newTask" class="block text-md mb-2">New Task</label>
      <input
        @input="error = ''"
        type="text"
        id="newTask"
        v-model="newTask"
        class="w-full p-2 border border-gray-300 rounded"
        :class="{ 'border-red-400 shadow-lg': error }"
        placeholder="Enter your task here"
      />
      <span v-if="error" class="text-red-400 text-sm">{{ error }}</span>
    </div>
    <div class="text-end">
      <button
        type="submit"
        class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
      >
        Add Task
      </button>
    </div>
  </form>
</template>
