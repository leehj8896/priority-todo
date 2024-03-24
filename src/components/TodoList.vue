<template>
  <div class="todo-list">
    <div
      v-for="(item, i) in todoList"
      v-bind:key="`todo${i}`"
      v-bind:class="`todo-item ${item.selected ? 'selected' : ''}`"
      v-on:click="selectItem(item)"
    >
      <p class="todo-priority">{{ item.priority === 0 ? '' : item.priority }}</p>
      <p v-bind:class="`todo-title ${item.done ? 'done': ''}`">{{ item.title }}</p>
      <input type="checkbox" v-model="item.done">
      <button class="update" v-on:click="updateItem(item)">수정</button>
      <button class="remove" v-on:click="removeItem(item)">삭제</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'TodoList',
  props: {
    todoList: [],
  },
  created() {
    console.log('TodoList created')
    console.log(`this.todoList: ${this.todoList}`)
  },
  methods: {
    removeItem(todoItem) {
      console.log('TodoList removeItem')
      console.log(`todoItem`, todoItem)
      this.$emit('remove-todo', todoItem)
    },
    selectItem(todoItem) {
      console.log('TodoList selectItem')
      console.log(todoItem)
      this.todoList.forEach((item) => {
        if (item.idx === todoItem.idx) {
          item.selected = !todoItem.selected
        } else {
          item.selected = false
        }
      });
    },
    updateItem(todoItem) {
      console.log('TodoList > updateItem')
      console.log(todoItem)
      this.$emit('update-item', todoItem)
    },
  }
}
</script>

<style>
.todo-list {
  width: 100%;
  height: calc(100% - 50px);
  border: 2px solid;
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  align-content: center;
  gap: 0px;
}
.todo-item {
  border: 1px solid grey;
  height: 60px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.todo-item .todo-priority {
  text-align: center;
  flex-basis: 10%;
}
.todo-item .todo-title {
  text-align: center;
  flex-basis: 40%;
}
.update {
  width: 40px;
  height: 40px;
}
.remove {
  width: 40px;
  height: 40px;
  margin-right: 10px;
}
.done {
  text-decoration: line-through;
  color: grey;
}
.selected {
  background-color: rgb(189, 189, 255);
}
</style>
