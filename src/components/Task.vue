<script setup lang="ts">
import deleteSVG from "../assets/deleteSVG.vue";
import checkBoxcheckSVG from "../assets/checkBox-check-SVG.vue";
import checkBoxUncheckSVG from "../assets/checkBox-uncheck-SVG.vue";
import pencilSVG from "../assets/pencilSVG.vue";
import cross from "../assets/cross.vue";

import { TaskType } from "../assets/types";

import { ref, useTemplateRef } from "vue";

type Props = {
  task: TaskType;
  index: number;
  setLocalStorageCheckBox: (index: number) => void;
  setLocalStorageDiscription: (index: number, discription: string) => void;
  deleteTask: (index: number) => void;
};

const { task, setLocalStorageDiscription } = defineProps<Props>();

const isOnChecked = ref<boolean>(task.isChecked);
const isEdited = ref<boolean>(false);
const className = ref<string>(task.isChecked ? "task complite-task" : "task");
const discription = ref<string>(task.discription);

const input = useTemplateRef<HTMLInputElement>("input");

function chengeCheckBox(): void {
  isOnChecked.value = !isOnChecked.value;
  className.value = isOnChecked.value ? "task complite-task" : "task";
}

function editTask(): void {
  isEdited.value = !isEdited.value;
}

function updateTask(): void {
  if (input.value?.value === "") {
    input.value?.classList.add("error");
    return;
  }
  input.value?.classList.remove("error");

  discription.value = input.value ? input.value.value : discription.value;
}

function stopEdit(e: Event, index: number) {
  e.preventDefault();
  isEdited.value = !isEdited.value;
  setLocalStorageDiscription(index, discription.value);
}
</script>

<template>
  <div :class="className">
    <div class="info">
      <form @submit="(e) => stopEdit(e, index)" v-if="isEdited">
        <input ref="input" @input="updateTask()" :value="discription" />
      </form>
      <p v-else>{{ discription }}</p>
      <p>{{ task.date }}</p>
    </div>
    <div class="buttons">
      <button
        @click="(e) => (isEdited ? stopEdit(e, index) : editTask())"
        class="image-button"
      >
        <cross v-if="isEdited" />
        <pencilSVG v-else />
      </button>
      <button
        class="image-button"
        @click="
          () => {
            chengeCheckBox();
            setLocalStorageCheckBox(index);
          }
        "
      >
        <checkBoxcheckSVG v-if="isOnChecked" />
        <checkBoxUncheckSVG v-else />
      </button>
      <button @click="deleteTask(index)" class="image-button">
        <deleteSVG />
      </button>
    </div>
  </div>
</template>

<style scoped>
.task {
  color: white;
  position: relative;
  display: flex;
  width: 90%;
  border-radius: 8px;
  padding: 15px 5%;
  background-color: #7e64ff;
  margin-bottom: 15px;
}

.buttons {
  position: absolute;
  right: 0;
  top: 0;
}

.image-button {
  background-color: transparent;
  width: 25px;
  height: 25px;
  border: none;
  border-radius: 0;
  margin: 5px 5px 0 0;
  padding: 0;
}
.image-button:focus {
  outline: none;
}

.info {
  display: flex;
  flex-direction: column;
}

p {
  margin: 0;
}
</style>
