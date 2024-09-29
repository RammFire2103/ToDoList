<script setup lang="ts">
type Props = {
  addTask: (e: Event) => void;
};

defineProps<Props>();

function removeClass(e: Event): void {
  const el = e.target as HTMLInputElement;
  console.log(el.classList.remove("error"));
}
</script>

<template>
  <form>
    <input @input="(e) => removeClass(e)" placeholder="Введите задачу" />
    <button @click="(e) => addTask(e)">Добавить</button>
  </form>
</template>

<style scoped>
form {
  display: flex;
  justify-content: space-between;

  width: 100%;
  margin-bottom: 30px;
}

input {
  width: 80%;
  padding-left: 5%;

  border: 2px solid transparent;
  border-radius: 30px 0 0 10px;

  font-family: Roboto;
  font-size: 1em;

  transition: 500ms;
}
input:focus {
  outline: none;
}

button {
  width: 20%;
  transition: 500ms;
  background-color: #827f8a;
  border: 2px solid transparent;
}
button:hover {
  transition: 500ms;
  background-color: #5a595e;
}
</style>
