<script setup>
import { ref, computed } from 'vue';

const todos = ref([]);
const newTodo = ref("");
const hideCompleted = ref(false);
const editingTodoId = ref(null);
const previousTodoText = ref("");
let nextId = 0;

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter(todo => !todo.done) : todos.value;
});

function addTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({ id: nextId++, text: newTodo.value, done: false });
    newTodo.value = "";
  }
}

function removeTodo(todo) {
  todos.value = todos.value.filter(t => t !== todo);
}

function startEditing(todo) {
  editingTodoId.value = todo.id;
  previousTodoText.value = todo.text;
}

function updateTodoText(todo) {
  if (todo.text.trim()) {
    todo.text = todo.text.trim();
    editingTodoId.value = null;
  }
}

function cancelEditing(todo) {
  todo.text = previousTodoText.value;
  editingTodoId.value = null;
}

function toggleHideCompleted() {
  hideCompleted.value = !hideCompleted.value;
}
</script>

<template>
  <div class="main">
    <div class="container">
      <div class="header">
        <h1>App To Do List</h1>
        <form @submit.prevent="addTodo">
          <input v-model="newTodo" required placeholder="Add a new todo" class="input-new-todo" />
          <button type="submit" class="button-add">Add Todo</button>
        </form>
        <button @click="toggleHideCompleted" class="button-toggle">
          {{ hideCompleted ? 'Show Completed' : 'Hide Completed' }}
        </button>
      </div>
      <div class="todo-list-box">
        <ul>
          <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
            <div class="todo-content">
              <input type="checkbox" v-model="todo.done" class="checkbox" />
              <span v-if="editingTodoId !== todo.id" :class="{ done: todo.done }">{{ todo.text }}</span>
              <input v-show="editingTodoId === todo.id" class="edit-input" type="text" v-model="todo.text"
                @keyup.enter="updateTodoText(todo)" @keydown.esc="cancelEditing(todo)" />
            </div>
            <div class="todo-actions">
              <button @click="removeTodo(todo)" class="button-remove">Remove</button>
              <button @click="startEditing(todo)" class="button-edit">Edit</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

 
<style>
body, button {
  margin: 0;
  padding: 0;
  border: none;
  background: none;
  font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
  vertical-align: baseline;
  color: #333;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  background-image: url("./assets/1.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  margin: 0 auto;
  max-width: 550px;
  padding: 20px;
}

/* Headings */
h1 {
  color: #b83f45;
  font-size: 36px;
  font-weight: 300;
  text-align: center;
  margin-bottom: 20px;
}

/* Form and Input Styling */
.input-new-todo {
  width: calc(100% - 110px); /* Adjusted input width */
  padding: 10px;
  margin-right: 10px; /* Added margin */
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  font-size: 18px;
  color: #666;
}

.input-new-todo:focus {
  border-color: #88aaff;
}

.button-add {
  padding: 8px 16px;
  background-color: #5cacf7;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s;
}

.button-add:hover {
  background-color: #4488cc;
}

/* List Styling */
.todo-item {
  background-color: #fff;
  margin-top: 8px;
  padding: 10px;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 1px 3px rgba(0,0,0,0.2);
}

.todo-item:nth-child(even) {
  background-color: #f8f8f8;
}

.todo-content {
  display: flex;
  align-items: center;
}

.checkbox {
  margin-right: 10px;
}

.todo-actions button {
  margin-left: 8px;
}

.todo-list-box {
  background-color: #f7f7f7;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 10px;
}

/* Toggle and Edit Buttons */
.button-toggle, .button-remove, .button-edit {
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s;
}

.button-toggle {
  width: 100%;
  margin-top: 12px;
  background-color: #f7f7f7;
  border: 1px solid #ccc;
}

.button-remove {
  background-color: #f66;
}

.button-edit {
  background-color: #6cf;
}

/* Responsive Design Adjustments */
@media (max-width: 600px) {
  body {
    padding: 10px;
  }

  .input-new-todo {
    width: calc(100% - 50px); /* Adjusted input width */
  }

  .todo-item {
    flex-direction: column;
    align-items: flex-start;
  }

  .button-toggle {
    margin-top: 8px; /* Adjusted margin for button-toggle */
  }
}

/* Accessibility Enhancements */
:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(130, 180, 255, 0.5);
}
</style>