<template>
  <div class="add-todo">
    <input class="input-text" type="text" v-model="inputTitle">
    <input class="input-priority" type="text" v-model="priority">
    <input class="add-btn" type="button" value="Add" v-on:click="addTodo">
  </div>
</template>

<script>

export default {
  name: 'AddTodo',
  components: {
  },
  data() {
    return {
      inputTitle: '',
      priority: '',
    }
  },
  methods: {
    isNumericString(str) {
      return /^\d+$/.test(str);
    },
    addTodo() {
      console.log('AddTodo: onClickAdd')
      if (this.inputTitle && this.isNumericString(this.priority)) {
        this.$emit('add-todo', {
          title: this.inputTitle,
          priority: parseInt(this.priority),
        })
        this.inputTitle = ''
        this.priority = ''
      }
    },
  },
}
</script>

<style>
.add-todo {
  width: 100%;
  height: 30px;
  border: 1px solid blue;
  margin-left: auto;
  margin-right: auto;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.input-text {
  flex-basis: 80%;  
}
.input-priority {
  width: 40px;
  padding: 0;
}
.add-btn {
  width: 40px;
}
</style>
