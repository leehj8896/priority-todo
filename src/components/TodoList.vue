<template>
  <div class="todo-list">
    <div class="todo-item"
      v-for="(item, i) in todoList"
      v-bind:key="`todo${i}`"
    >
      <p>{{ item.title }}</p>
      <input type="checkbox" v-if="item.checked" checked>
      <input type="checkbox" v-else>
      <button class="remove" v-on:click="removeItem(item)">X</button>
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
    }
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
  gap: 3px;
}
.todo-item {
  border: 1px solid red;
  height: 60px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.todo-item p {
  text-align: center;
  flex-basis: 50%;
}
.remove {
  width: 40px;
  height: 40px;
  margin-right: 10px;
}
</style>
