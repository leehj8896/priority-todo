<template>
  <div class="container">
    <div class="title">
      <p>My Todo List</p>
    </div>
    <div class="main-content">
      <add-todo v-on:add-todo="addTodo"/>
      <todo-list
        v-bind:loading="loading"
        v-bind:todo-list="showedTodoList"
        v-on:remove-todo="removeTodo"
        v-on:update-item="updateItem"
        v-on:scroll-to-end="onScrollToEnd"
        v-bind:is-search-mode="isSearchMode"
      />
      <search-todo
        v-bind="isSearchMode"
        v-on:search-title="onClickSearchTitle"
      />
    </div>
    <loading-spinner v-if="loading"/>
    <update-popup
      v-if="showPopup"
      v-bind:current-todo-item="currentTodoItem"
      v-on:cancel-update="cancelUpdate"
      v-on:submit-update="submitUpdate"
    />
    <login-popup
      v-if="!isLoggedin"
      v-on:success-login="onSuccessLogin"
    />
  </div>
</template>

<script>
import AddTodo from './components/AddTodo.vue'
import axios from 'axios'
import TodoList from './components/TodoList.vue'
import UpdatePopup from './components/UpdatePopup.vue'
import SearchTodo from './components/SearchTodo.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'
import LoginPopup from './components/LoginPopup.vue'

export default {
  name: 'App',
  components: {
    TodoList,
    AddTodo,
    UpdatePopup,
    SearchTodo,
    LoadingSpinner,
    LoginPopup,
  },
  data() {
    return {
      loading: false,
      showPopup: false,
      todoList: [],
      currentTodoItem: {},
      isSearchMode: false,
      searchTitle: '',
      itemsPerPage: 10,
      currentPage: 1,
      isLoggedin: false,
      curreutUser: {},
    }
  },
  computed: {
    showedTodoList() {
      if (!this.isSearchMode) return this.todoList
      return this.todoList.filter((item) => item.title.includes(this.searchTitle))
    }
  },
  watch: {
    isLoggedin: {
      immediate: true,
      handler() {
        console.log('App > onChangeLoginState')
        if (this.isLoggedin) {
          this.setTodoList()
        }
      },
    },
  },
  created() {
    console.log('App > created')
  },
  methods: {
    async setTodoList() {
      console.log('App > setTodoList')
      this.loading = true
      const response = await axios.get('http://localhost:3000/todos', {
        params: {
          userId: this.curreutUser.id,
          _sort: 'priority',
          _limit: this.itemsPerPage * this.currentPage,
        },
      })
      console.log('get todos response', response.data)
      this.todoList = response.data
      this.todoList = this.todoList.sort((a, b) => a.priority - b.priority)
      setTimeout(() => {
        this.loading = false
      }, 500)
    },
    async onScrollToEnd() {
      console.log('App > onScrollToEnd')
      if (await this.existMoreData()) {
        this.currentPage++
        this.setTodoList()
      }
    },
    async existMoreData() {
      console.log('App > existMoreData')
      const { data } = await axios.get('http://localhost:3000/todos', {
        params: {
          userId: this.curreutUser.id,
        }
      })
      return data.length > this.todoList.length
    },
    async addTodo(todoInput) {
      console.log('App: addTodo')
      console.log(`todoInput: ${todoInput}`)
      const { title, priority } = todoInput
      if (!title) return
      const newItem = {
        title,
        priority,
        done: false,
        selected: false,
        userId: this.curreutUser.id,
      }
      const response = await axios.post('http://localhost:3000/todos', newItem)
      console.log('post todos response', response.data)
      if (this.itemsPerPage * this.currentPage < this.todoList.length + 1) {
        this.currentPage++
      }
      // this.insertItem(newItem)
      // console.log('this.todoList', this.todoList)
      this.setTodoList()
    },
    insertItem(newItem) {
      console.log('App > insertItem', newItem) 
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
    async removeTodo(todoItem) {
      console.log('App removeTodo')
      console.log('todoItem', todoItem)
      const response = await axios.delete(`http://localhost:3000/todos/${todoItem.id}`)
      console.log('delete todo response', response.data)
      this.setTodoList()
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
    async submitUpdate(updateTodoInfo) {
      console.log('App > submitUpdate', updateTodoInfo)
      const response = await axios.patch(`http://localhost:3000/todos/${updateTodoInfo.id}`, updateTodoInfo)
      console.log('patch todo response', response.data)
      // this.todoList = this.todoList.map((todo) => {
      //   if (updateTodoInfo.id === todo.id) {
      //     return {
      //       ...todo,
      //       ...updateTodoInfo,
      //     }
      //   }
      //   return todo
      // })
      this.setTodoList()
      this.showPopup = false
    },
    onClickSearchTitle(title) {
      console.log('App > searchTitle', title)
      console.log('isSearchMode', this.isSearchMode)
      this.isSearchMode = !this.isSearchMode
      if (this.isSearchMode) {
        this.searchTitle = title
      }
    },
    onSuccessLogin(userInfo) {
      console.log('App > onSuccessLogin')
      this.isLoggedin = true
      this.curreutUser = userInfo
    },
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
