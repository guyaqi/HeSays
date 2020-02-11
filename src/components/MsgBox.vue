<template>
  <div class="msg-box-root">
    <div class="msg" @click="showMask=true">{{text}}</div>
    <div v-show="showMask" class="box-mask" @click="showMask=false">
      <textarea class="mask-text" v-model="text" @click.stop></textarea>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: '我什么都没说过!',
      showMask: false,
    }
  }
}
</script>

<style>
.msg {
  font-size: 45px;
  color: black;
  padding: 35px;
  background: white;
  border-radius: 40px;
  border: none;
  position: relative;
  
  margin-top: 10px;
  margin-left: 20px;
  max-width: 80%;
}
.msg::placeholder {
  color: black;
}
.box-mask {
  position: absolute;
  z-index: 1;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.25);
  display: flex;
  justify-content: center;
  align-items: center;
  
}

.mask-text {
  color: white;
  font-size: 42px;
  background: transparent;

  border: 7px dashed lightsalmon;
  padding: 30px;
  /* border-radius: 13px; */
  width: 80%;
  height: 30%;
}
.msg::before {
  position: absolute;
  z-index: 1;
  content: "";
  height: 25px;
  width: 20px;
  top: 23px;
  left: -18px;
  background-image: url("../assets/deg.svg")
}
</style>