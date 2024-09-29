<script setup lang="ts">
import AddForm from "./components/AddForm.vue";
import Task from "./components/Task.vue";

import { setItem, getItem, TaskType } from "./assets/types";

import { ref } from "vue";

//Массив todo-шек

const tasks = ref<TaskType[]>([]);
const arr = getItem<TaskType[]>("tasks");
if (arr) {
  tasks.value = arr;
}

function dateToString(date: Date): string {
  const dateStr: string =
    date.getFullYear() + "/" + (date.getMonth() + 1) + "/" + date.getDate();
  return dateStr;
}

//Изменение чекбокса в localstorage

function setLocalStorageCheckBox(index: number): void {
  tasks.value[index].isChecked = !tasks.value[index].isChecked;
  setItem<TaskType[]>("tasks", tasks.value);
}

//Изменение discription в localstorage

function setLocalStorageDiscription(index: number, discription: string): void {
  tasks.value[index].discription = discription;
  setItem<TaskType[]>("tasks", tasks.value);
}

//Получение todo-шки из формы

function addTask(e: Event) {
  e.preventDefault();
  const el = e.target as HTMLButtonElement;
  if (el.form) {
    if ((el.form[0] as HTMLInputElement).value === "") {
      el.form[0].classList.add("error");
      return;
    }

    tasks.value.push({
      discription: (el.form[0] as HTMLInputElement).value,
      date: dateToString(new Date()),
      isChecked: false,
      index: tasks.value[0] ? tasks.value[tasks.value.length - 1].index + 1 : 0,
    });
    setItem<TaskType[]>("tasks", tasks.value);
    (el.form[0] as HTMLInputElement).value = "";
  }
}

//Удаление todo-шки

function deleteTask(index: number): void {
  tasks.value.splice(index, 1);
  setItem<TaskType[]>("tasks", tasks.value);
}
</script>

<template>
  <div class="container">
    <AddForm :addTask="addTask" />
    <Task
      v-for="(el, index) in tasks"
      :task="el"
      :key="el.index"
      :index="index"
      :setLocalStorageCheckBox="setLocalStorageCheckBox"
      :setLocalStorageDiscription="setLocalStorageDiscription"
      :deleteTask="deleteTask"
    />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;

  width: 50%;
  margin: 0 auto;
  padding-top: 30px;
}
</style>
