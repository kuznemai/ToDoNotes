<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue';

const props = defineProps({
  id: Number,
  title: String,
  isChecked: Boolean
})

const emit = defineEmits(['deleteItem','sendCheckboxState','sendNewText'])

function deleteItem() {
  emit('deleteItem', props.id)
}


const checkedState = ref(props.isChecked)

watch(() => checkedState.value, (newValue) => {
  checkedState.value = newValue
})

function sendCheckboxState(){
  emit('sendCheckboxState', { id: props.id, isChecked: checkedState.value })
}

function handleCheckboxChange(){
  checkedState.value = !checkedState.value
  sendCheckboxState()
}


const isEditing = ref(false)



function editNote(){
  isEditing.value = true;
  console.log('editNote')
}
function stopEditingNote(){
  isEditing.value = false;
  sendText()
  console.log('stopEditingNote')

}

const innerText = ref(props.title);

function sendText(){
  emit ('sendNewText', props.id, innerText.value)
}

// watch(innerText.value, (newVal) => innerText.value = newVal)

</script>

<template>
  <li class="list-item">
    Я заметка номер {{ props.id }}
    <p v-if="!isEditing" @click="editNote" class="big">{{ innerText }}</p>
    <input
        v-else
        @blur="stopEditingNote"
        @keydown.enter="stopEditingNote"
        v-model="innerText"
        type="text"
        class="edit-input"
        placeholder="Введите вашу заметку"
    />
    <form>
      <input
          type="checkbox"
          id="checkbox"
          :checked="checkedState"
          @change="handleCheckboxChange"
      />
      <label>Done</label>
    </form>
    <button @click="deleteItem" class="delete">Delete</button>
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
  height: 300px;
  border: 1px solid #000000;
  border-radius: 20px;
  gap: 20px;
}

.big {
  width: 180px;
  height: 50px;
  border: 1px solid grey;
  font-size: 24px;
}

.delete {
  background-color: red;
  color: white;
  width: 60px;
  height: 30px;
  border: 1px solid darkred;
  border-radius: 10px;
}

.edit-input {
  font-size: 16px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>