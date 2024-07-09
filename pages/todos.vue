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
  <ul style="display: flex; flex-direction: column; align-items: center">
    <v-col
      cols="12"
      md="6"
      lg="3"
      style="display: flex; justify-content: center"
    >
      <v-text-field label="Todo" v-model="todoText"></v-text-field>
      <v-btn @click="addTodo()" style="height: 55px">Add</v-btn>
    </v-col>
    <li v-for="todo in todos" style="display: flex; align-items: center; gap: 1rem;">
      {{ todo.id }}
      {{ todo.name }}
      <v-select
        v-model="todo.status"
        label="Select"
        :items="['Pending', 'Doing', 'Done']"
        style="width: 300px;"
      />
      <v-btn @click="editTodo(todo.id, todo)">Update</v-btn>
    </li>
  </ul>
</template>
