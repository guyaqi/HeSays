<template>
  <div class="log-root">
    <div class="tabs">
      <div
        v-for="(item, index) in logs"
        :key="index"
        class="tab"
        :class="{'tab-active': logIndex==index}"
        @click="logIndex=index"
        >{{item.title}}</div>
    </div>
    <div class="log-area">
      {{logs[logIndex].content}}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      logIndex: 0,
      logs: [
        {
          title: "Log 1",
          content: "(Log 1)Nothing..."
        },
        {
          title: "Log 2",
          content: "(Log 2)Nothing..."
        },
      ]
    }
  }
}
</script>

<style>
.log-root {
  background: white;
  width: 100%;
  font-size: 30px;
  color: lightsalmon;
}
.tabs {
  display: flex;
  border-bottom: 2px solid lightsalmon;
}
.tab {
  padding: 10px 30px;
  border-right: 2px solid lightsalmon;
  margin: 0 3px;
}
.tab-active {
  font-weight: bold;
}
.log-area {
  padding: 30px;
}
</style>