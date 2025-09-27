<script setup lang="ts">

import type {TaskFilter} from "../types.ts";

defineProps<{
  filter: TaskFilter,
  currentFilter: TaskFilter
}>();

defineEmits<{
  setFilter: [filter: TaskFilter],
}>();

</script>

<template>

  <button
      :class="{
        contrast: currentFilter === filter
      }"
      class="secondary"
      @click="$emit('setFilter', filter)">{{ filter }}
  </button>

</template>

<style scoped>

button{
  text-transform: capitalize;
}

</style>