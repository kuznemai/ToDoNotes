<script setup>
import {ref} from 'vue';

const currentDate = ref(new Date());

const text = ref('Купить макаронов');
const isEditing = ref(false)

const formattedDate = ref(currentDate.value.toLocaleDateString('ru-RU', {
  year: 'numeric',
  month: 'long',
  day: 'numeric'
}));

function editText() {
  isEditing.value = true
}

function stopEditing() {
  isEditing.value = false
}


</script>

<template>

  <li class="list-item">
    <p v-if="!isEditing" @click="editText" class="big">{{ text }}</p>
    <input
        v-else
        type="text"
        class="edit-input"
        v-model="text"
        @blur="stopEditing"
        @keydown.enter="stopEditing">
    <form>
      <input type="checkbox" id="checkbox1">
      <label for="checkbox1">Done</label>
    </form>
    <p>{{ formattedDate }}</p>
    <div class="status">
      <div class="circle"></div>
      <p class="done small">Выполняется</p>
    </div>
    <button class="delete">Delete</button>
  </li>


</template>

<style>

.wrapper {
  display: flex;
}

.list-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  width: 200px;
  height: 250px;
  border: 1px solid #000000;
  border-radius: 20px;
}

.status {
  display: flex;
  align-items: center;
  width: 100px;
  height: 20px;
  border: 1px solid darkgreen;
  border-radius: 10px;
  padding: 5px;
}

.circle {
  width: 10px;
  height: 10px;
  background-color: darkgreen;
  border-radius: 5px;
  margin-right: 5px;
}

.done {
  color: darkgreen;
}

.calcelled {
  color: red;
}

.pending {
  color: orange;
}

.small {
  font-size: 12px;
}

.big {
  font-size: 24px;
}

input[type="checkbox"] {
  accent-color: #17880f;
  width: 20px;
  height: 20px;
}

.delete {
  width: 70px;
  height: 30px;
  margin: 10px;
  border: 1px solid red;
  border-radius: 10px;
  background-color: red;
  color: beige;
}

.edit-input {
  font-size: 16px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
