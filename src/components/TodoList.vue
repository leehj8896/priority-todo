<template>
  <div class="todo-list">
    <div class="todo-item"
      v-for="(item, i) in todoList"
      v-bind:key="`todo${i}`"
    >
      <p>{{ item.title }}</p>
      <input type="checkbox" v-if="item.checked" checked>
      <input type="checkbox" v-else>
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
  }
}
</script>

<style>
.todo-list {
  width: 90%;
  height: 500px;
  margin: auto;
  border: 2px solid;
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  align-content: center;
  gap: 3px;
}
.todo-item {
  border: 1px solid red;
  height: 60px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
.todo-item p {
  text-align: center;
}
</style>
