<template>
  <div class="sharepage">
    <Header />
    <div class="inputstyle">
      <van-field v-model.trim="value" center clearable placeholder="请输入 建筑 设计 等">
        <template #button>
          <van-button size="small" type="primary" @click="onshare" block>天橙 搜索</van-button>
        </template>
      </van-field>
    </div>
    <p class="bottomtip">天橙·有光浏览器</p>
  </div>
</template>
<script>
import Header from '@/components/Header'
export default {
  components: {
    Header
  },
  data() {
    return {
      value: ''
    }
  },
  methods: {
    onshare() {
      if (!this.value) {
        this.$toast('请输入内容')
        return
      }
      window.open(`https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=${this.value}`)
    }
  }
}
</script>
<style lang="scss">
.sharepage {
  padding-top: 230px;
  width: 100%;
  min-height: 100vh;
  box-sizing: border-box;
  //   background: #fff;
  .inputstyle {
    padding: 0 20px;
  }
  .bottomtip {
    position: absolute;
    bottom: 0;
    width: 100%;
    text-align: center;
    color: #cecece;
    padding: 20px;
    box-sizing: border-box;
  }
}
</style>
