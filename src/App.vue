<script setup lang="ts">
import { ref, computed } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import type { Task } from "./types";
import type { TaskFilter } from "./types";
import FilterButton from "./components/FilterButton.vue";

const message = ref("My Tasks");
const tasks = ref<Task[]>([]);
const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0)
);

const filter = ref<TaskFilter>("all");

function setFilter(newFilter: TaskFilter) {
  filter.value = newFilter;
}

const filteredTasks = computed(() => {
  if (filter.value === "all") {
    return tasks.value;
  } else if (filter.value === "done") {
    return tasks.value.filter((task) => task.done);
  } else if (filter.value === "pending") {
    return tasks.value.filter((task) => !task.done);
  }
  return tasks.value;
});

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function toggleDone(id: string) {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(id: string) {
  const index = tasks.value.findIndex((task) => task.id === id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }
}
</script>

<template>
  <main class="container max-w-4xl mx-auto p-4">
    <h1 class="text-5xl font-bold mb-4 text-center text-blue-500">
      {{ message }}
    </h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length" class="text-xl text-red-400">
      No tasks available!
    </h3>
    <div v-else class="flex justify-between mb-3 mt-5">
      <h3 class="text-xl font-bold mb-3">
        {{ totalDone }} / {{ tasks.length }} Completed
      </h3>
      <div class="flex gap-1">
        <FilterButton
          filter="done"
          :current-filter="filter"
          @update-filter="setFilter"
        />
        <FilterButton
          filter="pending"
          :current-filter="filter"
          @update-filter="setFilter"
        />
        <FilterButton
          filter="all"
          :current-filter="filter"
          @update-filter="setFilter"
        />
      </div>
    </div>
    <TaskList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>
