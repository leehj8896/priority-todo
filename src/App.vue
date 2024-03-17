<template>
  <div class="container">
    <div class="title">
      <p>My Todo List</p>
    </div>
    <div class="main-content">
      <add-todo v-on:add-todo="addTodo"/>
      <todo-list v-bind:todo-list="todoList" v-on:remove-todo="removeTodo"/>
    </div>
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
        idx: this.todoList.length > 0
          ? Math.max(...this.todoList.map((item) => item.idx)) + 1
          : 1,
        title: inputValue,
        done: false
      })
      console.log('this.todoList', this.todoList)
    },
    removeTodo(todoItem) {
      console.log('App removeTodo')
      console.log('todoItem', todoItem)
      this.todoList = this.todoList.filter((item) => item.idx !== todoItem.idx)
      console.log('this.todoList', this.todoList)
    }
  },
}
</script>

<style>
body {
  margin: 0;
  overflow: hidden;
}
div, p {
  box-sizing: border-box;
}
.container {
  width: 100vw;
  height: 100vh;
}
.title {
  width: 100%;
  height: 20%;
  background-color: rgb(255, 197, 110);
  border: 2px solid black;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.title p {
  text-align: center;
  font-size: 3rem;
}
.main-content {
  padding: 10px;
  width: 100%;
  height: 80%;
}
.input-section {
  box-sizing: border-box;
  width: 90%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 10px;
}

</style>
