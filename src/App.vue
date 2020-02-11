<template>
  <div class="root" @touchmove="move" @touchend="end">
    <div class="main" id="sc-area">
      <img class="avatar" :src="avatarUrl" />
      <div class="right">
        <input class="nickname" type="text" placeholder="(请输入昵称)" v-model="nickname" />
        <MsgBox></MsgBox>
      </div>
    </div>
    <div class="btn-group">
      <button class="btn" @click="screenCut">📷✨✨</button>
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

    <Log v-show="debug"></Log>

    <ImageView
      v-show="showScreenShot"
      :src="screenShot"
      @close="showScreenShot=false"
      ></ImageView>
  </div>
</template>

<script>
import Log from './components/Log'
import ImageView from './components/ImageView'
import html2canvas from 'html2canvas'
import MsgBox from './components/MsgBox'

export default {
  components: {
    Log,
    ImageView,
    MsgBox
  },
  data() {
    return {
      nickname: '受害者',
      sizing: false,
      lastX: 0,
      lastY: 0,
      oriList: [
        require('./assets/ztl_avatar.jpg'),
        require('./assets/gyq_avatar.jpg'),
        require('./assets/wk_avatar.jpg'),
      ],
      cachedList: [],
      avatarIndex: 0,
      screenShot: '',
      showScreenShot: false,
      debug: false,
    }
  },
  computed: {
    avatarList() {
      return this.oriList.concat(this.cachedList)
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
    },
    screenCut() {
      this.showScreenShot = true
      let that = this
      html2canvas(document.querySelector('#sc-area')).then(function(canvas) {
        that.screenShot = canvas.toDataURL()
          // document.body.appendChild(canvas);
      });
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
  margin-right: 0;
  font-size: 32px;
  height: 80px;
  line-height: 15px;
  padding: 0 30px;
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