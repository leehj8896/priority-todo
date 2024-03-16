<template>
  <div class="container">
    <div class="title">
      <p>My Todo List</p>
    </div>
    <add-todo v-on:add-todo="addTodo"/>
    <todo-list v-bind:todo-list="todoList"/>
  </div>
</template>

<script>
import AddTodo from './components/AddTodo.vue'

import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList,
    AddTodo,
  },
  data() {
    return {
      todoList: [],
    }
  },
  methods: {
    addTodo(inputValue) {
      console.log('App: addTodo')
      console.log(`inputValue: ${inputValue}`)
      if (!inputValue) return
      this.todoList.push({
        title: inputValue,
        done: true
      })
      console.log(`this.todoList: ${JSON.stringify(this.todoList)}`)
    }
  },
}
</script>

<style>
body {
  margin: 0;
  overflow: hidden;
}
.container {
}
.title {
  margin: auto;
  background-color: rgb(255, 197, 110);
  border: 2px solid black;
}
.title p {
  text-align: center;
  font-size: 3rem;
}
.input-section {
  box-sizing: border-box;
  width: 90%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 10px;
}

</style>
