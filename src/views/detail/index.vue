<template>
  <div class="detail">
    <Header />
    <div class="iframestyle">
      <iframe class="iframe" :src="detail.url" frameborder="0" @load="onMyFrameLoad"></iframe>
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
      detail: '',
      toast: ''
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
      this.toast.clear()
      this.share()
    },
    share() {
      let detail = this.detail
      let wxConfig = {
        title: detail.title,
        url: location.href,
        desc: detail.desc || '',
        link: window.location.href + '&time=' + new Date().getTime(),
        imgUrl: detail.cover
      }
      setShareInfo(wxConfig)
    },

    goHome() {
      this.$router.replace('/')
    },
    getList() {
      let id = this.$route.query.id
      if (!id) {
        this.$router.replace('/')
      }
      this.toast = this.$toast.loading({
        duration: 0, // 持续展示 toast
        forbidClick: true,
        message: '加载中'
      })
      getShareArticleDetails({ article_id: id }).then(res => {
        this.detail = res.data
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.detail {
  position: relative;
  .iframe {
    width: 100%;
    min-height: 100vh;
  }
  .button {
    position: fixed;
    bottom: 60px;
    right: 23px;
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
