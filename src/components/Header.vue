<template>
  <div class="headerTop">
    <div class="header">
      <div class="headericon">
        <span class="iconfont icon-caidan" @click="goHome"></span>
      </div>
      <h5>天橙·有光</h5>
      <div class="headericon">
        <span class="iconfont icon-sousuo" @click="goShare"></span>
        <span class="iconfont icon-shouye" @click="goHome"></span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Header',
  methods: {
    goShare() {
      if (this.$route.path == '/share') {
        //   this.$toast("")
        return
      }
      this.$router.push('/share')
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

<style lang="scss">
.headerTop {
  .header {
    position: fixed;
    top: 0;
    padding: 8px 0;
    box-sizing: border-box;
    width: 100%;
    background: #fff;
    /* padding: 10px 0; */
    color: #4a4a4a;
    z-index: 99;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .headericon {
      width: 70px;
    }
    span {
      font-size: 28px;
      padding: 0 3px;
    }
    h5 {
      text-align: center;
      font-size: 18px;
      letter-spacing: 8px;

      text-indent: 8px;
    }
  }
}
</style>
