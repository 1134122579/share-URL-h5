<template>
  <div class="sharepage">
    <Header />
    <div class="inputstyle">
      <h3>天空之橙·有光</h3>
      <van-field v-model.trim="value" center clearable placeholder="请输入 建筑 设计 等">
        <template #button>
          <van-button size="small" type="primary" @click="onshare" color="#4e6ef2" block>有光 搜索</van-button>
        </template>
      </van-field>
    </div>
    <p class="bottomtip">有光·搜索·浏览器1.0</p>
  </div>
</template>
<script>
import Header from '@/components/Header'
import { setShareInfo } from '@/utils/share'

export default {
  components: {
    Header
  },
  data() {
    return {
      value: ''
    }
  },
  created() {
    let wxConfig = {
      title: '天空之橙·有光 | 搜索',
      url: location.href,
      desc: '',
      //   link: location.href,
      link: window.location.href,
      imgUrl: 'http://api.skyorange.cn/logo.jpg'
    }
    setShareInfo(wxConfig)
  },
  methods: {
    onshare() {
      if (!this.value) {
        this.$toast('请输入内容')
        return
      }
      window.open(`https://www.baidu.com/s?word=${this.value}`)
    }
  }
}
</script>
<style lang="scss" scoped>
.sharepage {
  width: 100%;
  min-height: 100vh;
  box-sizing: border-box;
  //   background: #fff;
  .inputstyle {
    padding: 0 20px;
    padding-top: 110px;
    h3 {
      font-size: 22px;
      text-align: center;
      padding: 60px 0;
    }
  }
  .bottomtip {
    position: absolute;
    bottom: 0;
    font-size: 12px;
    width: 100%;
    text-align: center;
    color: #cecece;
    padding: 20px 0;
    box-sizing: border-box;
  }
}
</style>
