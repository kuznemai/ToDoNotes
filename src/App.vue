<script setup>
import ToDoItem from './components/ToDoItem.vue';
import { ref } from "vue";


let items = ref([
  { id: 1, title: 'Buy bread', isChecked: false },
  { id: 2, title: 'Buy sausage', isChecked: false },
  { id: 3, title: 'Buy butter', isChecked: false },
  { id: 4, title: 'Buy milk', isChecked: false }
])

function removeItem(itemId){
  items.value = items.value.filter(elem => elem.id !== itemId)
}
function handleCheckbox({id, isChecked}){
  const item = items.value.find(item => item.id === id)
    if(item){
    item.isChecked = isChecked
  }
}
const textFromChild = ref('');

function getNewText(id, title) {
  console.log('Received:', id, title);
  const item = items.value.find(item => item.id === id);
  if (item) {
    item.title = title;
    textFromChild.value = title;
  }
}
function addNewNote(){
  items.value.push({ id: (items.value.length + 1), title: '', isChecked: false})
}

</script>

<template>
  <div class="wrapper">
    <div class="inprocess">
      <ul class="items_list">
        <ToDoItem
            v-for="item in items.filter(item => !item.isChecked)"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :isChecked="item.isChecked"
            @deleteItem="removeItem"
            @sendCheckboxState="handleCheckbox"
            @sendNewText="getNewText"
        />
      </ul>
      <ul class="right_list">
        <ToDoItem
            v-for="item in items.filter(item=> item.isChecked)"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :isChecked="item.isChecked"
            @deleteItem="removeItem"
            @sendCheckboxState="handleCheckbox"
            @sendNewText="getNewText"

        />
      </ul>
    </div>
  </div>
  <button @click="addNewNote" class="addbutton">Добавить заметку</button>
</template>

<style>
.wrapper {
  width: 100%;
}

.items_list {
  width: 45%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 15px;
  border-right: 5px dashed grey;
}

.right_list {
  width: 45%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 15px;
}

.inprocess {
  width: 95%;
  display: flex;
  justify-content: space-between;

}

.done {
  width: 45%;
}

.addbutton {
  width: 150px;
  height: 40px;
  background-color: #17880f;
  border-radius: 10px;
  color: beige;
  font-size: 16px;
  margin: 20px;
}
</style>