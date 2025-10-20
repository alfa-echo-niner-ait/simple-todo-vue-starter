<script lang="ts" setup>
import type { TaskFilter } from "../types";

const porps = defineProps<{
  filter: TaskFilter;
  currentFilter: TaskFilter;
}>();

const classes = {
  all: "bg-blue-100 hover:bg-blue-300 text-blue-800 text-xs me-2 px-2.5 py-0.5 rounded-full",
  done: "bg-green-100 hover:bg-green-300 text-green-800 text-xs me-2 px-2.5 py-0.5 rounded-full",
  pending:
    "bg-red-100 hover:bg-red-300 text-red-800 text-xs me-2 px-2.5 py-0.5 rounded-full",
};

const emits = defineEmits<{
  updateFilter: [filter: TaskFilter];
}>();
</script>

<template>
  <button
    @click="emits('updateFilter', porps.filter)"
    :class="[
      classes[porps.filter],
      { 'border shadow-lg font-bold': currentFilter == porps.filter },
    ]"
  >
    {{ porps.filter.charAt(0).toUpperCase() + porps.filter.slice(1) }}
  </button>
</template>
