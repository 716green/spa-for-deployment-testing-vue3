<template>
  <section class="todo-component">
    <span>
      <h3>{{ titleAndCount }}</h3>
    </span>
    <section class="todos-wrapper">
      <div class="todos">
        <ul class="list" v-for="todo in todos" :key="todo.id">
          <li class="list-item-wrapper">
            <input class="checkbox" v-model="todo.complete" type="checkbox" />
            <div
              :class="todo.archived ? 'list-item archived' : 'list-item'"
              @click="$emit('toggleTodo', todo.id)"
            >
              {{ todo.name }}
            </div>

            <button class="trash" @click="$emit('deleteTodo', todo.id)">
              🗑
            </button>
          </li>
        </ul>
      </div>
    </section>
  </section>
</template>

<script>
  export default {
    name: 'Todos',
    props: {
      title: {
        type: String,
        default: '',
      },
      todos: {
        type: Array,
        default: () => [],
      },
    },
    computed: {
      titleAndCount() {
        return this.todos.length !== 0
          ? this.title + ' - ' + this.todos.length
          : this.title;
      },
    },
  };
</script>

<style scoped>
  input.checkbox {
    min-width: 20px;
    width: 20px;
    height: 20px;
    margin-right: 15px;
  }
  button.trash {
    cursor: pointer;
    padding-left: 10px;
    border: none;
  }
  .todos-wrapper {
    justify-content: center;
    display: flex;
    margin: 20px 10px;
  }
  .todos {
    border: 1px solid lightgray;
    border-radius: 0.75rem;
    box-shadow: 1px 1px 2px lightgray;
    padding: 1rem;
    width: 500px;
  }
  .list {
    margin: 0;
    padding: 0;
  }
  .list-item-wrapper {
    display: flex;
  }
  .list-item {
    display: flex;
    text-align: left;
    align-content: baseline;
    cursor: pointer;
  }
  .todo-item {
    display: flex;
  }
  .archived {
    text-decoration: line-through;
    text-align: left;
  }
</style>
