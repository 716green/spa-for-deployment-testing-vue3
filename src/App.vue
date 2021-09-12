<template>
  <section class="app">
    <header>
      <div>
        <h1>{{ appTitle }}</h1>
        <label for="todoInput">{{ taskLabel }}</label>
      </div>
      <input
        id="todoInput"
        class="todo-input"
        type="text"
        v-model="currentTask"
        v-on:keypress.enter="addTodo"
        placeholder="Enter Task"
      />
    </header>
    <Todos
      v-show="incompleteTodos.length > 0"
      title="Incomplete"
      :todos="incompleteTodos"
      @toggleTodo="toggleTodo"
      @deleteTodo="deleteTodo"
    />
    <Todos
      v-show="completedTodos.length > 0"
      title="Complete"
      :todos="completedTodos"
      @toggleTodo="toggleTodo"
      @deleteTodo="deleteTodo"
    />
    <Todos
      v-show="archivedTodos.length > 0"
      title="Archived"
      :todos="archivedTodos"
      @toggleTodo="toggleTodo"
      @deleteTodo="deleteTodo"
    />
  </section>
</template>

<script>
  import Todos from './components/Todos.vue';
  import { sampleTodos } from './sampleTodos.js';

  export default {
    name: 'App',
    emits: ['toggleTodo:id', 'deleteTodo:id'],
    data() {
      return {
        inDevMode: false,
        currentTask: '',
        todos: [],
      };
    },
    mounted() {
      if (this.inDevMode) {
        sampleTodos.forEach((sampleTodo) => {
          this.todos.push(sampleTodo);
        });
      }
    },
    methods: {
      addTodo() {
        if (this.currentTask.trim()) {
          const todo = {
            id: this.nextId,
            name: this.currentTask.trim(),
            complete: false,
            archived: false,
          };
          this.todos.push(todo);
          this.currentTask = '';
        }
      },
      deleteTodo(id) {
        this.todos.forEach((todo) => {
          let i = 0;
          if (id === todo.id) {
            i = this.todos.indexOf(todo);
            this.todos.splice(i, 1);
          }
        });
      },
      toggleTodo(id) {
        this.todos.forEach((todo) => {
          let i = 0;
          if (id === todo.id) {
            i = this.todos.indexOf(todo);
            this.todos[i].archived = !this.todos[i].archived;
          }
        });
      },
    },
    computed: {
      completedTodos() {
        return this.nonArchivedTodos.filter((todo) => {
          return todo.complete;
        });
      },
      incompleteTodos() {
        return this.nonArchivedTodos.filter((todo) => {
          return !todo.complete;
        });
      },
      nonArchivedTodos() {
        return this.todos.filter((todo) => {
          return !todo.archived;
        });
      },
      archivedTodos() {
        return this.todos.filter((todo) => {
          return todo.archived;
        });
      },
      nextId() {
        if (this.todos?.length > 0) {
          let id = 0;
          this.todos.forEach((todo) => {
            if (todo.id > 0) {
              id = todo.id;
            }
          });
          return id + 1;
        } else {
          return 0;
        }
      },
      appTitle() {
        return this.todos.length !== 0
          ? 'Tasks' + ' - ' + this.todos.length
          : 'Tasks';
      },
      taskLabel() {
        return this.currentTask ? 'Press enter to submit' : 'Enter your task';
      },
    },

    components: {
      Todos,
    },
  };
</script>

<style>
  #app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: white;
    margin-top: 60px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 1.2rem;
  }
  input:focus {
    outline: none !important;
  }
  input.todo-input {
    margin-top: 10px;
    background-color: white;
    border: 0;
    outline: 0;
    border-radius: 0.25rem;
    height: 50px;
    font-size: 3rem;
    padding: 10px;
    width: 450px;
  }
</style>
