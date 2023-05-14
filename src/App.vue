<template>
  <div class="toDo">
    <div class="toDo__container">
      <h1 class="toDo__title">Simple To-do list</h1>
      <div class="toDo__taskInfo">
        <h2 class="toDo__desc">Your tasks: <span class="toDo__selectedTask">0</span> / <span class="toDo__count">{{tasksList.length}}</span></h2>
      </div>
      <div class="toDo__form">
        <input type="text" class="toDo__taskDesc" v-model="taskName" @keyup.enter="addTask">
        <button class="toDo__addBtn" @click="addTask">Add</button>
      </div>
      <div class="toDo__taskList">
        <ul class="toDo__taskWrapper" v-for="task in tasksList" :key="task">
          <li class="toDo__taskName" :id="task">
            <div class="toDo__taskContent">
              <input type="checkbox" class="toDo__check">
              <span class="toDo__taskText">{{ task }}</span>
            </div>
            <button class="toDo__remove" @click="removeTask">Remove</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  name: 'App',
  components: {},
  setup() {
    const taskName = ref();
    const addedTask = ref(false);
    const tasksList = ref([])

    //add new task
    function addTask() {
      if(taskName.value && !tasksList.value.includes(taskName.value)) {
        tasksList.value.unshift(taskName.value);
        taskName.value = '';
      }
    }

    //remove the task
    function removeTask(event) {
      tasksList.value = tasksList.value.filter(item => item !== event.target.parentElement.id)
      }

    return {
      taskName,
      addedTask,
      addTask,
      tasksList,
      removeTask
    }
  }
}
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
  align-items: center;
  margin: 0 auto;
  padding: 20px;

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
}
</style>
