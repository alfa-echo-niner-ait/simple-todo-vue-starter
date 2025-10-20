<script lang="ts" setup>
import type { Task } from "../types";

const props = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();
</script>

<template>
  <div class="mt-6">
    <transition-group name="list" tag="ul">
      <li
        v-for="task in props.tasks"
        :key="task.id"
        class="flex justify-between items-center p-4 border border-gray-200 rounded-lg mb-3 hover:bg-gray-100 hover:shadow"
      >
        <label>
          <input
            type="checkbox"
            @input="emits('toggleDone', task.id)"
            :checked="task.done"
            class="me-3"
          />
          <span :class="{ 'line-through text-gray-500': task.done }">
            {{ task.title }}
          </span>
        </label>
        <button
          @click="emits('removeTask', task.id)"
          class="text-sm font-bold border border-red-200 px-2 py-1 rounded bg-red-50 hover:bg-red-400 text-red-700 hover:text-white"
        >
          X
        </button>
      </li>
    </transition-group>
  </div>
</template>

<style>
.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-active {
  position: absolute;
}
</style>
