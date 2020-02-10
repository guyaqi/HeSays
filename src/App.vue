<template>
  <div class="root" @touchmove="move" @touchend="end">
    <div class="main">
      <img class="avatar" :src="avatarUrl" />
      <div class="right">
        <input class="nickname" type="text" placeholder="受害者" />
        <div class="msg-wrapper">
          <textarea class="msg" :style="textStyle">我什么都没说过!</textarea>
          <div
            class="control"
            @touchstart="start"
            v-show="showResizer"
            ></div>
        </div>
      </div>
    </div>
    <div class="btn-group">
      <button class="btn" v-show="!showResizer" @click="showResizer=true">show resizer</button>
      <button class="btn" v-show="showResizer" @click="showResizer=false">hide resizer</button>
    </div>
    
    <div class="avatar-list">
      <img
        v-for="(one, index) in avatarList"
        :key="index"
        class="avatar inlist"
        :src="one"
        @click="avatarIndex = index">
      <div class="upload-wrapper">
        <input class="upload-hide" accept=".png,.jpg,.jpeg" type="file" @change="getFile"/>
        <img class="avatar" src="./assets/avatar_new.svg" alt="">
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      th: 500,
      tw: 500,
      sizing: false,
      lastX: 0,
      lastY: 0,
      showResizer: false,
      oriList: [
        require('./assets/ztl_avatar.jpg'),
        require('./assets/lbl_avatar.jpg'),
      ],
      cachedList: [],
      avatarIndex: 0,
    }
  },
  computed: {
    avatarList() {
      return this.oriList.concat(this.cachedList)
    },
    textStyle() {
      return {
        height: this.th + 'px',
        width: this.tw + 'px'
      }
    },
    avatarUrl() {
      return this.avatarList[this.avatarIndex]
    }
  },
  created() {
    let cache = localStorage.getItem('avatar')
    if (cache != undefined) {
      this.cachedList = JSON.parse(cache)
    }
  },
  methods: {
    start(e) {
      console.log('s')
      e = e.touches[0]
      this.sizing = true
      this.lastX = e.clientX
      this.lastY = e.clientY
    },
    move(e) {
      console.log(e)
      if (this.sizing) {
        e = e.touches[0]
        let dx = e.clientX - this.lastX
        let dy = e.clientY - this.lastY

        this.th += dy
        this.tw += dx

        this.lastX = e.clientX
        this.lastY = e.clientY
      }
    },
    end(e) {
      this.sizing = false
    },
    getFile(event) {
      let file = event.target.files[0];
      console.log(file)

      let that = this;
      var a = new FileReader();
      a.onload = function(e) {
        that.cachedList.push(e.target.result);
        localStorage.setItem('avatar', JSON.stringify(that.cachedList))
      };
      a.readAsDataURL(file);

      // this.avatarList.push(file)
    }
  }
}
</script>

<style>
  body {
    margin: 0
  }
</style>

<style scoped>
.root {
  background-color: antiquewhite;
  width: 100vw;
  height: 100vh;
  
}
.main {
  background-color: #eaedf4;
  padding: 30px 30px 60px 30px;
  display: flex;
}
.avatar {
  height: 120px;
  width: 120px;
  border-radius: 60px;
}
.inlist {
  margin-right: 30px;
  margin-bottom: 30px;
}
.right {
  margin-left: 10px;
  display: flex;
  flex-flow: column;
}
.nickname {
  font-size: 32px;
  color: #888a97;
  border: none;
  background: transparent;
}
.msg {
  font-size: 45px;
  color: black;
  padding: 40px 35px;
  background: white;
  border-radius: 40px;
  
  border: none;
  
}
.msg::placeholder {
  color: black;
}
.msg-wrapper {
  position: relative;
  margin-top: 10px;
  margin-left: 20px;
}
.msg-wrapper::before {
  position: absolute;
  z-index: 1;
  content: "";
  height: 25px;
  width: 20px;
  top: 23px;
  left: -18px;
  background-image: url("./assets/deg.svg")
}
.control {
  height: 70px;
  width: 70px;
  background-color: red;
  position: absolute;
  right: -35px;
  bottom: -35px;
}
.btn {
  margin: 30px;
  font-size: 32px;
  padding: 15px;
  color:lightsalmon;
  border: none;
  border-radius: 13px;
  background-color: white;
}
.btn-group {
  background-color: #eaedf4;
}
.avatar-list {
  padding: 30px;
  display: flex;
  flex-flow: row wrap;
}
.upload-wrapper {
  position: relative;
}
.upload-hide {
  position: absolute;
  width: 120px;
  height: 120px;
  opacity: 0;
}
</style>