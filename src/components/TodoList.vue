<template>
  <div>
    <ul>
      <TodoItem
          v-for="(todo, index) of todos"
          v-bind:todo="todo"
          v-bind:index="index"
          v-bind:key="todo.id"
          v-on:remove-todo="removeTodo"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from '@/components/TodoItem';

export default {
  props: ['todos'],
  methods: {
    removeTodo(id) {
      this.$emit('remove-todo', id);
    },
  },
  name: 'TodoList',
  components: {TodoItem},
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
