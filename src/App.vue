<template>
  <div class="toDo">
    <div class="toDo__container">
      <h1 class="toDo__title">Simple To-do list</h1>
      <div class="toDo__taskInfo">
        <h2 class="toDo__desc">Your tasks: <span class="toDo__selectedTask">{{ count }}</span> / <span
            class="toDo__count">{{ tasksList.length }}</span></h2>
      </div>
      <div class="toDo__form">
        <input type="text" class="toDo__taskDesc" v-model="taskName" @keyup.enter="addTask">
        <button class="toDo__addBtn" @click="addTask">Add</button>
      </div>
      <div class="toDo__taskList">
        <ul class="toDo__taskWrapper" v-for="task in tasksList" :key="task.content">
          <li class="toDo__taskName" :id="task.content">
            <div class="toDo__taskContent">
              <input type="checkbox" @change="setCheck" :checked="task.checked" class="toDo__check">
              <span class="toDo__taskText" :class="{'toDo__taskText--done': task.checked}">{{ task.content }}</span>
            </div>
            <button class="toDo__remove" @click="removeTask">Remove</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import {ref, computed, onMounted} from 'vue';

const taskName = ref();
const tasksList = ref([]);
const isChecked = ref(false);

//add new task
function addTask() {
  let isAdded = tasksList.value.some(item => item.content === taskName.value);
  if (taskName.value && !isAdded) {
    tasksList.value.unshift({
      content: taskName.value,
      checked: false
    });
    taskName.value = '';
    localStorage.setItem('task', JSON.stringify(tasksList.value))
  }
}

//remove the task
function removeTask(event) {
  tasksList.value = tasksList.value.filter(item => item.content !== event.target.parentElement.id)
  localStorage.setItem('task', JSON.stringify(tasksList.value))
}

//change checked value of task
function setCheck(event) {
  if (event.target.checked) {
    isChecked.value = true
    for (let item of tasksList.value) {
      if (item.content === event.target.nextElementSibling.textContent) {
        item.checked = true;
        localStorage.setItem('task', JSON.stringify(tasksList.value))
      }
    }
  } else {
    isChecked.value = false
    for (let item of tasksList.value) {
      if (item.content === event.target.nextElementSibling.textContent) {
        item.checked = false;
        localStorage.setItem('task', JSON.stringify(tasksList.value))
      }
    }
  }
}

//count of checked tasks
const count = computed(() => {
  let checkCount = 0;
  for (let item of tasksList.value) {
    if (item.checked) {
      checkCount++
    }
  }
  return checkCount;
});
onMounted(() => {
  tasksList.value = JSON.parse(localStorage.getItem('task'))
})
</script>

<style lang="scss">
body {
  margin: 0;
}

.toDo {
  max-width: 700px;
  width: 100%;
  background: #ccc;
  display: flex;
  justify-content: center;
  margin: 0 auto;
  padding: 20px;
  height: 100vh;
  box-sizing: border-box;
  overflow: auto;

  &__container {
    width: 100%;
  }

  &__taskDesc {
    background: #fff;
    border: 1px solid #000;
    border-radius: 8px;
    padding: 10px 15px;
    max-width: 200px;
    width: 100%;
    margin-right: 15px;

    &:focus {
      outline: none;
    }
  }

  &__addBtn {
    padding: 10px;
    border: none;
    background: darkgray;
    font-weight: 700;
    color: blue;
    border-radius: 8px;
    cursor: pointer;
  }

  &__taskName {
    list-style: none;
    padding: 15px;
    margin: 0;
    background: lightsteelblue;
    border-radius: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__taskWrapper {
    padding: 0;
  }

  &__remove {
    padding: 8px;
    border: none;
    background: red;
    border-radius: 8px;
    color: #fff;
    font-weight: 700;

    &:hover {
      background: #fff;
      outline: 1px solid red;
      color: red;
      cursor: pointer;
    }
  }

  &__taskText {
    &--done {
      text-decoration: line-through;
    }
  }
}
</style>
