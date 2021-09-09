<template>
  <div class="body">
    <h1>Todo List</h1>
    <div>
      <TodoForm />
    </div>
  </div>
</template>

<script>
import TodoForm from './components/TodoForm.vue'

export default {
  name: 'App',
  components: {
    TodoForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.body {
  max-width: 560px;
  margin: 20px auto;
}
</style>
