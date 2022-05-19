<template>
  <div class="headerTop">
    <div class="header">
      <div class="headericon">
        <span class="iconfont icon-caidan" @click="goHome"></span>
      </div>
      <h5>天空之橙·有光</h5>
      <div class="headericon">
        <span class="iconfont icon-sousuo2" @click="goShare"></span>
        <span class="iconfont icon-31shouyexuanzhong" @click="goHome"></span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Header',
  methods: {
    goShare() {
      if (this.$route.path == '/typelist') {
        //   this.$toast("")
        return
      }
      this.$router.push('/typelist')
    },
    gotypelist() {
      if (this.$route.path == '/typelist') {
        //   this.$toast("")
        return
      }
      this.$router.push('/typelist')
    },
    goHome() {
      if (this.$route.path == '/') {
        //   this.$toast("")
        return
      }
      this.$router.replace('/')
    }
  }
}
</script>

<style lang="scss" scoped>
.headerTop {
  .header {
    position: fixed;
    top: 0;
    box-sizing: border-box;
    width: 100%;
    background: #fff;
    padding: 6px 10px;
    color: #4a4a4a;
    z-index: 99;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .headericon {
      width: 58px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      color: #d8d8d8;
    }
    span {
      font-size: 18px;
      padding: 0 3px;
    }
    h5 {
      flex: 1;
      text-align: center;
      font-size: 16px;
      letter-spacing: 8px;
      text-indent: 8px;
      font-weight: normal;
    }
  }
}
</style>
