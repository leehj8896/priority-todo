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
    addTodo(todoInput) {
      console.log('App: addTodo')
      console.log(`todoInput: ${todoInput}`)
      const { title, priority } = todoInput
      if (!title) return
      const newItem = {
        idx: this.todoList.length > 0 ? 
          Math.max(...this.todoList.map((item) => item.idx)) + 1 :
          1,
        title,
        priority,
        done: false
      }
      if (this.todoList.length === 0) {
        this.todoList.push(newItem)
      } else {
        let finished = false
        for (let i = 0; i < this.todoList.length; i++) {
          if (this.todoList[i].priority < priority) {
            continue
          } else {
            this.todoList.splice(i, 0, newItem)
            finished = true
            break
          }
        }
        if (!finished) this.todoList.push(newItem)
      }
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
