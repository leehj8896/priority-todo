<template>
  <div class="search-container">
      <input type="text" name="" id="" class="input-search" v-model="searchTitle">
      <button class="search-btn" v-on:click="onClickSearch">
        {{ isSearchMode ? '취소' : '검색' }}
      </button>
  </div>
</template>

<script>

export default {
  name: 'SearchTodo',
  props: {
    isSearchMode: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      searchTitle: '',
    }
  },
  created() {
    console.log('SearchTodo created')
  },
  methods: {
    onClickSearch() {
      console.log('TodoList > onClickSearch', this.searchTitle)
      this.$emit('search-title', this.searchTitle)
      if (this.isSearchMode) this.searchTitle = ''
    },
  }
}
</script>

<style>
.search-container {
  width: 100%;
  height: 24px;
  border: 1px solid;
  position: sticky;
  bottom: 0;
}
.input-search {
  width: 80%;
  box-sizing: border-box;
}
.search-btn {
  width: 20%;
  height: 100%;
  overflow: hidden;
  box-sizing: border-box;
}
</style>
