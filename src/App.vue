<template lang="">
  <!-- Vue markup -->
  <div>
    <h1 class="text-3xl">Todo List</h1>
    <div>
      <form @submit.prevent="addTodo()">
        <h1 class="flex text-lg">New Todo:</h1>
        <input v-model="newTodo" name="newTodo" autocomplete="off"
          class="rounded p-4 border-t mr-0 border-1 text-gray-600 border-gray-200 bg-white"
          placeholder="Add todo item.."
        />
        <button
          class="bg-blue-500 hover:bg-green-500 text-white font-bold p-4 pb-3 border-b-4 border-blue-700 hover:border-green-700 rounded"
        >
          + Add
        </button>
      </form>
    </div>
    <ul class="flex flex-col justify-center">
      <li v-for="(todo, index) in todos" :key="index" class="mt-3 border-b-4 border-yellow-500">
        <input type="checkbox" :checked="todo.done" @change="doneTodo(todo)" />
        <span :class="{done: todo.done}" @click="doneTodo(todo)">{{todo.content}}</span>
        <button
          class="float-right bg-red-500 hover:bg-red-800 text-white font-bold p-1 pb-1 border-b-4 border-red-700 hover:border-red-500 rounded justify-center"
        @click="removeTodo(index)"
        >
          Remove
        </button>
      </li>
    </ul>
    
    <h4 v-if="todos.length === 0" class="mt-3 border-t-4 border-yellow-600">Empty List</h4>
  </div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>

<script>
import { ref } from "vue";
export default {
  name: "TodoList",
  setup() {
    const newTodo = ref("");
    const defaultData = [
      {
        done: false,
        content: "Add something",
      },
    ];
    const todesData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todesData);

    function addTodo() {
      if(newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        });
        newTodo.value = "";
      }
      saveData();
    }

    function doneTodo(todo) {
      todo.done = !todo.done
      saveData();
    }

    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      saveData,
      removeTodo,
    };
  },
};
</script>