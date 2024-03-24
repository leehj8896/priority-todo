<template>
  <div class="container">
    <div class="title">
      <p>My Todo List</p>
    </div>
    <div class="main-content">
      <add-todo v-on:add-todo="addTodo"/>
      <todo-list
        v-bind:todo-list="todoList"
        v-on:remove-todo="removeTodo"
        v-on:update-item="updateItem"/>
    </div>
    <update-popup
      v-if="showPopup"
      v-bind:current-todo-item="currentTodoItem"
      v-on:cancel-update="cancelUpdate"
      v-on:submit-update="submitUpdate"
    />
  </div>
</template>

<script>
import AddTodo from './components/AddTodo.vue'

import TodoList from './components/TodoList.vue'
import UpdatePopup from './components/UpdatePopup.vue'

export default {
  name: 'App',
  components: {
    TodoList,
    AddTodo,
    UpdatePopup,
  },
  data() {
    return {
      showPopup: false,
      todoList: [],
      currentTodoItem: {},
    }
  },
  methods: {
    getNewItem(todoInfo) {
      return {
        idx: this.todoList.length > 0 ? 
          Math.max(...this.todoList.map((item) => item.idx)) + 1 :
          1,
        done: false,
        selected: false,
        ...todoInfo,
      }
    },
    addTodo(todoInput) {
      console.log('App: addTodo')
      console.log(`todoInput: ${todoInput}`)
      const { title, priority } = todoInput
      if (!title) return
      const newItem = this.getNewItem(todoInput)
      this.insertItem(newItem)
      console.log('this.todoList', this.todoList)
    },
    insertItem(newItem) {
      if (this.todoList.length === 0) {
        this.todoList.push(newItem)
      } else {
        let finished = false
        for (let i = 0; i < this.todoList.length; i++) {
          if (this.todoList[i].priority < newItem.priority) {
            continue
          } else {
            this.todoList.splice(i, 0, newItem)
            finished = true
            break
          }
        }
        if (!finished) this.todoList.push(newItem)
      }
    },
    removeTodo(todoItem) {
      console.log('App removeTodo')
      console.log('todoItem', todoItem)
      this.todoList = this.todoList.filter((item) => item.idx !== todoItem.idx)
      console.log('this.todoList', this.todoList)
    },
    updateItem(todoItem) {
      console.log('App > updateItem')
      console.log(todoItem)
      this.currentTodoItem = todoItem
      this.showPopup = true
    },
    cancelUpdate() {
      console.log('App cancelUpdate')
      this.showPopup = false
    },
    submitUpdate(updateTodoInfo) {
      console.log('App > submitUpdate', updateTodoInfo)
      // 삭제
      this.removeTodo(updateTodoInfo)
      // 다시삽입
      const newTodoItem = this.getNewItem(updateTodoInfo)
      this.insertItem(newTodoItem)
      this.showPopup = false
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
