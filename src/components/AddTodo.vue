<template>
  <div class="add-todo">
    <input class="input-text" type="text" v-model="inputValue">
    <input class="add-btn" type="button" value="Add" v-on:click="onClickAdd">
  </div>
</template>

<script>

export default {
  name: 'AddTodo',
  components: {
  },
  data() {
    return {
      inputValue: ''
    }
  },
  methods: {
    onClickAdd() {
      console.log('AddTodo: onClickAdd')
      this.$emit('add-todo', this.inputValue)
      this.inputValue = ''
    }
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
.add-btn {
  flex-basis: 20%;
}
</style>
