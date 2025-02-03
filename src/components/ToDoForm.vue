<template>
  <form @submit.prevent="onSubmit">
    <h2 class="label-wrapper">
      <label for="new-todo-input" class="label__lg">
        Что нужно сделать?
      </label>
    </h2>
    <input
      type="text"
      id="new-todo-input"
      name="new-todo"
      autocomplete="off"
      v-model.lazy.trim="label"
      class="input__lg" />
    <button type="submit" class="btn btn__primary btn__lg">Добавить</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
const emit = defineEmits(['todo-added']);
const label = ref("");

function onSubmit() {
  if (label.value === "") {
    return
  }
  emit("todo-added", label.value)
  label.value = "";
}
</script>
