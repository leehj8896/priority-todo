<template>
  <div class="popup-container">
    <!-- <div class="popup-background"></div> -->
    <div class="update-popup">
      <div class="popup-row">
        <p class="input-title">할 일</p>
        <input
          class="input-update-info"
          type="text"
          v-model="updateTodoInfo.title"
        >
      </div>
      <div class="popup-row">
        <p class="input-title">우선순위</p>
        <input
          class="input-update-info"
          type="text"
          v-model="updateTodoInfo.priority"
        >
      </div>
      <div class="popup-btn-container">
        <button class="cancel" v-on:click="onClickCancel">취소</button>
        <button class="submit" v-on:click="onClickSubmit">확인</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'UpdatePopup',
  props: {
    currentTodoItem: {},
  },
  data() {
    return {
    }
  },
  computed: {
    updateTodoInfo() {
      return {
        idx: this.currentTodoItem.idx,
        title: this.currentTodoItem.title,
        priority: this.currentTodoItem.priority,
        done: this.currentTodoItem.done,
        selected: this.currentTodoItem.selected,
      }
    },
  },
  methods: {
    onClickCancel() {
      console.log('UpdatePopup > onClickCancel')
      this.$emit('cancel-update')
    },
    onClickSubmit() {
      console.log('UpdatePopup > onClickSubmit')
      // validation 추가
      this.$emit('submit-update', {
        idx: this.updateTodoInfo.idx,
        title: this.updateTodoInfo.title,
        priority: parseInt(this.updateTodoInfo.priority),
        done: this.updateTodoInfo.done,
        selected: this.updateTodoInfo.selected,
      })
    },
  }
}
</script>

<style>
p {
  margin: 0;
}
.popup-container {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
.update-popup {
  width: 50%;
  height: 100px;
  border: 1px solid black;
  background-color: white;
  position: relative;
}
.popup-row {
  display: flex;
  flex-direction: row;
}
.input-title {
  font-size: 0.8rem;
  flex-basis: 30%;
}
.input-update-info {
  width: 80px;
  flex-basis: 70%;
}
.popup-btn-container {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: end;
  position: absolute;
  bottom: 0;
}
</style>
