<script setup>
import { useFetch } from "nuxt/app";

const BASE_URL = "https://668cd9de099db4c579f098d2.mockapi.io";

const todos = ref([]);
const todoText = ref("");

const loadTodo = async () => {
  const { data } = await useFetch(`${BASE_URL}/todos`);

  todos.value = data.value;
};

const addTodo = async () => {
  try {
    await useFetch(`${BASE_URL}/todos`, {
      method: "post",
      body: {
        name: todoText.value,
        status: "Pending",
      },
    });
    await loadTodo();
  } catch (error) {
    console.log("error", error);
  }
};

const editTodo = async (todoId, todoData) => {
  try {
    await useFetch(`${BASE_URL}/todos/${todoId}`, {
      method: "put",
      body: {
        status: todoData.status,
      },
    });
    await loadTodo();
  } catch (error) {
    console.log("error", error);
  }
};

loadTodo();
</script>

<template>
  <ul>
    <div>
      <input type="text" v-model="todoText" />
      <button @click="addTodo()">Add</button>
    </div>
    <li v-for="todo in todos">
      {{ todo.id }}
      {{ todo.name }}
      <select v-model="todo.status">
        <option>Pending</option>
        <option>Doing</option>
        <option>Done</option>
      </select>
      <v-btn @click="editTodo(todo.id, todo)">Update</v-btn>
    </li>
  </ul>
</template>
