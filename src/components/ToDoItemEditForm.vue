<template>
  <form class="stack-small" @submit.prevent="onSubmit">
    <div>
      <label class="edit-label">Edit Name for &quot;{{ label }}&quot;</label>
      <input
        :id="id"
        ref="labelInputRef"
        type="text"
        autocomplete="off"
        v-model.lazy.trim="newLabel" />
    </div>
    <div class="btn-group">
      <button type="button" class="btn" @click="onCancel">
        Cancel
        <span class="visually-hidden">editing {{ label }}</span>
      </button>
      <button type="submit" class="btn btn__primary">
        Save
        <span class="visually-hidden">edit for {{ label }}</span>
      </button>
    </div>
  </form>
</template>

<script setup>
import { ref, defineProps, defineEmits, onMounted } from 'vue';

const emits = defineEmits(['item-edited', 'edit-cancelled']);
const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  id: {
    type: String,
    required: true,
  }
});

const newLabel = ref(props?.label);
function onSubmit() {
  if (newLabel.value && newLabel.value !== props?.label) {
    emits("item-edited", newLabel.value);
  } else {
    onCancel();
  }
}

function onCancel() {
  emits("edit-cancelled");
}

const labelInputRef = ref(null);
onMounted(() => {
  labelInputRef.value.focus();
})
</script>

<style scoped>
.edit-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
input {
  display: inline-block;
  margin-top: 0.4rem;
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}
form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
form > * {
  flex: 0 0 100%;
}
</style>
