<template>
  <div class="image-view-root">
    <div class="loading" v-show="!src">加载中...</div>
    <img class="screen-shot" v-show="src" :src="src" alt="screen shot">
    <img class="close" @click="$emit('close')" src="../assets/close.svg" alt="">
  </div>
</template>

<script>
export default {
  data() {
    return {

    }
  },
  props: ['src']
}
</script>

<style>
.image-view-root {
  position: absolute;
  z-index: 1000;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.25);

  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}
.screen-shot {
  width: 80%;
}
.close {
  margin-top: 30px;
  height: 120px;
  width: 120px;
  border-radius: 60px;
}
.loading {
  color: white;
  font-size: 42px;
}
</style>