<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue';

const props = defineProps({
  todo: Object
})

const emit = defineEmits(['deleteItem','sendCheckboxState','sendNewText'])

function deleteItem() {
  emit('deleteItem', props.todo.id)
}


const checkedState = ref(props.todo.isChecked)

watch(() => checkedState.value, (newValue) => {
  checkedState.value = newValue
})

function sendCheckboxState(){
  emit('sendCheckboxState', { id: props.todo.id, isChecked: checkedState.value })
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

const innerText = ref(props.todo.title);

function sendText(){
  emit ('sendNewText', props.todo.id, innerText.value)
}

// watch(innerText.value, (newVal) => innerText.value = newVal)

</script>

<template>

    Я заметка номер {{ props.todo.id }}
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

</template>

<style>

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