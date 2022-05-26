<template>
  <div class="typedetail">
    <Header />
    <div class="iframestyle">
      <iframe class="iframe" v-if="detail.url" :src="detail.url" frameborder="0" @load="onMyFrameLoad"></iframe>
    </div>

    <div class="button" @click="goHome">
      <van-icon name="wap-home-o" size="20" color="#fff" />
    </div>
  </div>
</template>
<script>
import Header from '@/components/Header'
import { setShareInfo } from '@/utils/share'
import { getShareArticleDetails } from '@/api/user'
export default {
  components: {
    Header
  },
  data() {
    return {
      detail: {},
      toast: '',
      getData: ''
    }
  },
  created() {
    this.getList()
  },

  methods: {
    goShare() {
      this.$router.push('/share')
    },
    onMyFrameLoad() {
      //   this.toast.clear()
      this.share()
    },
    async share() {
      console.log(this.getData, 'getDatagetDatagetDatagetData')
      let wxConfig = {
        title: '天空之橙·有光 | 设计 建筑 空间',
        url: location.href,
        desc: '',
        link: window.location.origin + '/shareurl',
        imgUrl: 'http://api.skyorange.cn/logo.jpg'
      }
      setShareInfo(wxConfig)
    },
    goHome() {
      this.$router.replace('/')
    },
    getList() {
      let urltext = this.$route.query.urltext
      if (!urltext) {
        this.$router.replace('/typelist')
      }
      //   this.toast = this.$toast.loading({
      //     duration: 0, // 持续展示 toast
      //     forbidClick: true,
      //     message: '加载中'
      //   })
      this.$set(this.detail, 'url', urltext)
      this.share()
      return
      getShareArticleDetails({ article_id: id }).then(res => {
        this.detail = res.data
        this.getData = res.data
        this.share()
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.typedetail {
  position: relative;
  .iframe {
    width: 100%;
    min-height: 100vh;
  }
  .button {
    position: fixed;
    bottom: 70px;
    right: 16px;
    width: 50px;
    height: 50px;
    z-index: 999;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background: #767676;
    box-shadow: 0 0 8px #767676;
  }
}
</style>
