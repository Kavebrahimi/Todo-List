<script setup lang="ts">

import {computed, ref} from "vue";
import TaskForm from "./components/TaskForm.vue";
import type {Task, TaskFilter} from "./types.ts";
import TaskList from "./components/TaskList.vue";
import FilterBtn from "./components/FilterBtn.vue";

const message = ref("Tasks App");
const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");


const totalDone = computed(()=> tasks
    .value
    .reduce((total, task) => task.done ? total + 1 : total,0)
);

const filteredTasks = computed(()=> {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "done":
      return tasks.value.filter((task)=> task.done);
    case "todo":
      return tasks.value.filter((task)=> !task.done);
  }
  return tasks.value;
});

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false
  });
}

function toggleDone(id: string) {
  const task = tasks.value.find((task)=> task.id === id);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(id: string) {
  const index = tasks.value.findIndex((task) => task.id === id );
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }
}

function setFilter(value: TaskFilter) {
  filter.value = value
}

</script>

<template>

  <main>
    
    <h1>{{ message }}</h1>
    <TaskForm @addTask="addTask"/>
    <h3 v-if="!tasks.length">Add Task to get Started.</h3>
    <h3 v-else> {{ totalDone }} / {{ tasks.length }} Tasks Compeleted.</h3>
    <!--    Filter Btns     -->
    <div v-if="tasks.length" class="button-container">
      <FilterBtn
          :currentFilter="filter"
          filter="all"
          @setFilter="setFilter"/>
      <FilterBtn
          :currentFilter="filter"
          filter="todo"
          @setFilter="setFilter"/>
      <FilterBtn
          :currentFilter="filter"
          filter="done"
          @setFilter="setFilter"/>
    </div>
    <div></div>
    <TaskList :tasks="filteredTasks" @toggleDone="toggleDone" @removeTask="removeTask"/>

  </main>

</template>

<style>

  main{
    max-width: 800px;
    padding: 2rem;
    margin: 1rem auto;
  }

  .button-container{
    display: flex;
    justify-content: end;
    gap: .5rem;
  }

</style>