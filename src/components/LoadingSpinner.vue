<template>
  <div class="loading-container">
    <div class="loading-message">
      <p>로딩중...</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'LoadingSpinner',
  props: {
  },
  data() {
    return {
    }
  },
  computed: {
  },
  methods: {
  }
}
</script>

<style>
p {
  margin: 0;
}
.loading-container {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: grey;
  opacity: 0.8;
}
.update-popup {
  width: 50%;
  height: 100px;
  border: 1px solid black;
  background-color: white;
  position: relative;
}
</style>
