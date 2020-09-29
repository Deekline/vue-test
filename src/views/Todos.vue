<template>
  <div>
    <h2>Todo</h2>
    <router-link to="/">To Home page</router-link>
    <TodoAdd @add-todo="addTodo"/>
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not Completed</option>
    </select>
    <hr/>
    <Loader v-if="loading"/>
    <TodoList
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos"
        @remove-todo='removeTodo'/>
    <p v-else>No Todos</p>
  </div>
</template>

<script>
import TodoAdd from '@/components/TodoAdd';
import TodoList from '@/components/TodoList';
import Loader from '@/components/Loader';

export default {
  name: 'Todos',
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all',
    };
  },
  async mounted() {
    try {
      const resp = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=3');
      this.todos = await resp.json();
      this.loading = false;
    } catch (e) {
      console.error(e.message);
    }
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'all':
          return this.todos;
        case 'completed':
          return this.todos.filter(todo => todo.completed);
        case 'not-completed':
          return this.todos.filter(todo => !todo.completed);
        default:
          return this.todos;
      }
    },
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo) {
      this.todos.push(newTodo);
    },
  },
  components: {
    TodoAdd,
    TodoList,
    Loader,
  },
};

</script>

<style scoped>

</style>
