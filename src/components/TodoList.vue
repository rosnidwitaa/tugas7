<template>
    <div class="todo-list">
      <h1>Todo List</h1>
      <div class="input-container">
        <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add new todo" />
        <button @click="addTodo">Add</button>
      </div>
      <ul>
        <li v-for="(todo, index) in todos" :key="index" :class="{ done: todo.done }">
          <span @click="toggleTodo(index)">{{ todo.text }}</span>
          <button @click="removeTodo(index)">Delete</button>
        </li>
      </ul>
      <p>Total unfinished tasks: {{ unfinishedTodosCount }}</p>
    </div>
  </template>
  
  <script>
  import { useTodoStore } from '../stores/todoStore';
  import { computed, ref } from 'vue';
  
  export default {
    setup() {
      const todoStore = useTodoStore();
      const newTodo = ref('');
  
      const addTodo = () => {
        if (newTodo.value.trim() !== '') {
          todoStore.addTodo(newTodo.value);
          newTodo.value = '';
        }
      };
  
      const removeTodo = (index) => {
        todoStore.removeTodo(index);
      };
  
      const toggleTodo = (index) => {
        todoStore.toggleTodo(index);
      };
  
      return {
        newTodo,
        todos: computed(() => todoStore.todos),
        unfinishedTodosCount: computed(() => todoStore.unfinishedTodosCount),
        addTodo,
        removeTodo,
        toggleTodo
      };
    }
  };
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    background-color: #f7f7f7;
  }
  
  h1 {
    font-size: 1.8em;
    margin-bottom: 20px;
    color: #333;
    text-align: center;
  }
  
  .input-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  input {
    flex: 1;
    padding: 10px;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  button {
    padding: 10px 20px;
    background-color: #462064;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  li {
    display: flex;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #ddd;
  }
  
  li:last-child {
    border-bottom: none;
  }
  
  .done {
    text-decoration: line-through;
    color: #999;
  }
  
  button.delete {
    margin-left: auto;
    background-color: #ff4d4d;
  }
  
  p {
    margin-top: 20px;
    text-align: center;
    font-size: 1.2em;
    color: #777;
  }
  </style>
  