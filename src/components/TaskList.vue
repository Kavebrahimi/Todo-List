<script setup lang="ts">

import type {Task} from "../types.ts";

const props = defineProps<{
  tasks: Task[];
}>();

const emit = defineEmits<{
  toggleDone: [id: string];
  removeTask : [id: string];
}>();

</script>

<template>

  <TransitionGroup name="tasks-list" tag="div" class="tasks-list">
    <article v-for="task in props.tasks" :key="task.id">
      <label>
        <input
            :checked="task.done"
            @change="emit('toggleDone', task.id)"
            type="checkbox"
        >
        <span :class="{done: task.done}">{{ task.title }}</span>
      </label>
      <button @click="emit('removeTask', task.id)" class="outline">Remove</button>
    </article>
  </TransitionGroup>

</template>

<style>

  .tasks-list{
    margin-top: 1rem;
  }
  .done {
    text-decoration: line-through;
    color: lightskyblue;
  }
  article{
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-align: center;
  }

  .tasks-list-enter-active,
  .tasks-list-leave-active {
    transition: all 0.5s ease;
  }
  .tasks-list-enter-from,
  .tasks-list-leave-to {
    opacity: 0;
    transform: translateX(300px);
  }

</style>