<template>
  <div>
    <header>
      <h1>Vue Todo with Typescript</h1>
    </header>
    <main>
      <TodoInput v-bind="{item:todoText, temp:123}" @input="updateTodoText" @add="addTodoItem" />
      <!-- <TodoInput v-model="todoText" @add="addTodoItem" /> -->

      <div>
        <ul>
          <TodoListIem v-for="(todoItem, index) in todoItems" 
          :key="index" 
          :index="index" 
          :todoItem="todoItem" 
          @remove="removeTodoItem"
          @toggle="toggleTodoItem" />
          <!-- <li>Item 1</li>
          <li>Item 2</li>
          <li>Item 3</li> -->
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import { component } from "node_modules/vue/types/umd";
import Vue from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoListIem from "./components/TodoListIem.vue";

const STORAGE_KEY = "vue-todo-ts-v5";
const storage = {
  save(todoItems: Todo[]) {
    const parsed = JSON.stringify(todoItems)
    localStorage.setItem(STORAGE_KEY, parsed);
  },
  fetch() {
    const todoItems = localStorage.getItem(STORAGE_KEY) || '[]';
    const result = JSON.parse(todoItems);
    return result;
  }
};

export interface Todo {
  title: string;
  completed: boolean;
}

export default Vue.extend({
  components: { TodoInput, TodoListIem },
  data() {
    return {
      todoText: "",
      todoItems: [] as Todo[],
    }
  },
  methods: {
    updateTodoText(value: string) {
      this.todoText = value;
    },
    addTodoItem() {
      const value = this.todoText;
      // localStorage.setItem(STORAGE_KEY, value);
      const todo: Todo = {title: value, completed: false};
      this.todoItems.push(todo);
      storage.save(this.todoItems);
      this.initTodoText();
    },
    initTodoText() {
      this.todoText = '';
    },
    fetchTodoItems() {
      this.todoItems = storage.fetch();
    },
    removeTodoItem(index: number) {
      this.todoItems.splice(index, 1);
      storage.save(this.todoItems);
    },
    toggleTodoItem(index: number, todoItem: Todo) {
      this.todoItems.splice(index, 1, {
        ...todoItem,
        completed: !todoItem.completed
      });
      storage.save(this.todoItems);
    }
  },
  created() {
    this.fetchTodoItems();
  }
});
</script>
