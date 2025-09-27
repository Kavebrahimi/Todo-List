<script setup lang="ts">

import {ref} from "vue";

const newTask = ref("");
const error = ref("");
const emit = defineEmits<{

  addTask: [newTask: string]

}>();

function formSubmitted() {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value.trim());
    newTask.value = "";
  } else {
    error.value = "Task cannot be Empty!";
  }
}

</script>

<template>

  <!--    Form     -->
  <form @submit.prevent="formSubmitted">
    <label for="">New Task</label>
    <input
        v-model="newTask"
        type="text"
        name="newTask"
        :aria-invalid="!!error || undefined"
        @input="error='' "
    >
    <!--    Invalid Error    -->
    <small v-if="error" id="invalid-helper">{{ error }}</small>

    <!--    Button     -->
    <div class="button-container">
      <button>Add</button>
    </div>
  </form>

</template>

<style scoped>

</style>