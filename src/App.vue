<template>
  <div id="app">
    <h1>To-Do List</h1>
    <to-do-form @todo-added="addToDo"></to-do-form>
    <h2 id="list-summary" ref="listSummaryRef" tabindex="-1">{{ listSummary }}</h2>
    <ToDoSort @filter="filter"/>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in ToDoItemsFiltered" :key="item.id">
        <to-do-item
          :label="item.label"
          :done="item.done"
          :id="item.id"
          @checkbox-changed="updateDoneStatus(item.id)"
          @item-deleted="deleteToDo(item.id)"
          @item-edited="editToDo(item.id, $event)">
        </to-do-item>
      </li>
    </ul>
  </div>
</template>

<script setup>
import ToDoItem from "./components/ToDoItem.vue";
import ToDoForm from "./components/ToDoForm.vue";
import ToDoSort from './components/ToDoSort.vue';
import { nanoid } from "nanoid";
import { ref, computed } from 'vue';

const ToDoItems = ref(JSON.parse(localStorage.getItem("todos"))),
  filterBy = ref("All"),
  listSummaryRef = ref(null)

function filter(by) {
  filterBy.value = by
}
function addToDo(toDoLabel) {
  ToDoItems.value.push({
    id: "todo-" + nanoid(),
    label: toDoLabel,
    done: false,
  })

  localStorage.setItem("todos", JSON.stringify(ToDoItems.value))
}
function updateDoneStatus(toDoId) {
  const toDoToUpdate = ToDoItems.value.find((item) => item.id === toDoId)
  toDoToUpdate.done = !toDoToUpdate.done
  localStorage.setItem("todos", JSON.stringify(ToDoItems.value))
}
function deleteToDo(toDoId) {
  const itemIndex = ToDoItems.value.findIndex((item) => item.id === toDoId)
  ToDoItems.value.splice(itemIndex, 1)
  listSummaryRef.value.focus()
  localStorage.setItem("todos", JSON.stringify(ToDoItems.value))
}
function editToDo(toDoId, newLabel) {
  const toDoToEdit = ToDoItems.value.find((item) => item.id === toDoId)
  toDoToEdit.label = newLabel
  localStorage.setItem("todos", JSON.stringify(ToDoItems.value))
}

const listSummary = computed(() => {
  const numberFinishedItems = ToDoItems.value.filter(
    (item) => item.done,
  ).length
  return `${numberFinishedItems} из ${ToDoItems.value.length} задач ${numberFinishedItems === 1 ? "сделана" : "сделаны"}`
})

const ToDoItemsFiltered = computed(() => {
  if (filterBy.value === "Pending") {
    return ToDoItems.value.filter(todo => todo.done === false)
  } else if (filterBy.value === "Done") {
    return ToDoItems.value.filter(todo => todo.done === true)
  } else {
    return ToDoItems.value
  }
})
</script>

<style>
/* Global styles */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}
.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}
.btn__filter {
  border-color: lightgrey;
}
.btn__danger:focus {
  outline-color: #c82333;
}
.btn__primary {
  color: #fff;
  background-color: #000;
}
.btn-group {
  display: flex;
  justify-content: space-between;
}
.btn-group > * {
  flex: 1 1 auto;
}
.btn-group > * + * {
  margin-left: 0.8rem;
}
.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}
[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}
[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}
@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}
.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}
[class*="stack"] > * {
  margin-top: 0;
  margin-bottom: 0;
}
.stack-small > * + * {
  margin-top: 1.25rem;
}
.stack-large > * + * {
  margin-top: 2.5rem;
}
@media screen and (min-width: 550px) {
  .stack-small > * + * {
    margin-top: 1.4rem;
  }
  .stack-large > * + * {
    margin-top: 2.8rem;
  }
}
/* End global styles */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow:
    0 2px 4px 0 rgba(0, 0, 0, 0.2),
    0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}
@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}
#app > * {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}
#app > form {
  max-width: 100%;
}
#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}
</style>
