<script setup>
import ToDoItem from './components/ToDoItem.vue';
import {ref, computed} from "vue";


let items = ref([
  {id: 1, title: 'Buy bread', isChecked: false},
  {id: 2, title: 'Buy sausage', isChecked: false},
  {id: 3, title: 'Buy butter', isChecked: false},
  {id: 4, title: 'Buy milk', isChecked: false}
])

function removeItem(itemId) {
  items.value = items.value.filter(elem => elem.id !== itemId)
}

function handleCheckbox({id, isChecked}) {
  const item = items.value.find(item => item.id === id)
  if (item) {
    item.isChecked = isChecked
  }
}


function getNewText(id, title) {
  console.log('Received:', id, title);
  const item = items.value.find(item => item.id === id);
  if (item) {
    item.title = title;
  }
}

function addNewNote() {
  items.value.push({id: (Math.floor(Math.random() * (100 - 4 + 1) + 4)), title: '', isChecked: false})
}

// const groupedItems = computed(() => {
//   const checked = [];
//   const unchecked = [];
//
//   items.value.forEach(item => {
//     if (item.isChecked) {
//       checked.push(item);
//     } else {
//       unchecked.push(item);
//     }
//   });
//   return { checked, unchecked };
// });

const itemsList = computed(() => {
  return items.value.reduce((acc, elem) => {
    if (elem.isChecked) {
      acc[1].items.push(elem)
    } else {
      acc[0].items.push(elem)
    }
    return acc
  }, [{title: 'Not ready yet', items: [], class: 'items_list'}, {title: 'Ready', items: [], class: 'right_list'}])
})
console.log(itemsList.value)
</script>

<template>
  <div class="wrapper">
    <div class="inprocess">
      <template v-for="(group, groupIndex) in itemsList" :key="groupIndex">
        <h3 class="header_title">{{ group.title }}</h3>
        <ul :class="group.class">
          <li class="list-item" v-for="item in group.items" :key="item.id">
            <ToDoItem
                :todo="item"
                @delete-item="removeItem"
                @send-checkboxState="handleCheckbox"
                @send-new-text="getNewText"
            />
          </li>
        </ul>
      </template>
    </div>
    <button @click="addNewNote" class="addbutton">Добавить заметку</button>
  </div>
</template>

<style>
.wrapper {
  width: 100%;
}

.inprocess {
  width: 95%;
  gap: 10px;
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

.items_list {
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 15px;
  padding-bottom: 50px;
  border-bottom: 5px dashed grey;
}

.right_list {

  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 15px;
}

.header_title {
  font-size: 24px;
  width: 100%;
  height: 40px;
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