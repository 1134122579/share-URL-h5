<template>
  <div class="detailpage">
    <div class="title">来自天空之橙·Design | 空间设计</div>
    <div class="label">
      <p>建筑·空间·设计 的主题</p>
      <p class="time">{{ detail.create_time }}</p>
    </div>
    <van-divider :style="{ color: '#d2d2d2', borderColor: '#d2d2d2', margin: '0 8px' }" />
    <div class="content-text">
      {{ detail.title }}
    </div>
    <div class="content-imglist">
      <div class="imgstyle" v-for="(item, index) in detail.imgs">
        <img @click="ImagePreviewClick(index)" :src="item + '?imageView2/3/w/300'" alt="" />
      </div>
    </div>
    <van-divider :style="{ color: '#d2d2d2', borderColor: '#d2d2d2', padding: '16px 0', margin: '0 8px' }" />
    <!-- 横 -->
    <div class="h-imglist">
      <div class="h-img" v-for="(item, index) in detail.imgs">
        <img :src="item" @click="ImagePreviewClick(index)" alt="" />
      </div>
    </div>
    <!-- 返回 -->
    <div class="button" @click="goHome">
      <van-icon name="wap-home-o" size="20" color="#fff" />
    </div>
  </div>
</template>

<script>
import { getShareArticleDetails } from '@/api/user'
import { parseTime } from '@/utils'
import { ImagePreview } from 'vant'
import { setShareInfo } from '@/utils/share'
import { getUrlKey } from '@/utils/wxLoad'
export default {
  data() {
    return {
      detail: ''
    }
  },
  created() {
    this.getList()
    console.log(this.$route, 'this.$route.path.replace')
  },
  methods: {
    goHome() {
      this.$router.replace({ path: '/' })
    },
    ImagePreviewClick(index) {
      console.log(index)
      ImagePreview({
        images: this.detail.imgs,
        startPosition: index
      })
    },
    getList() {
      let article_id = this.$route.query.id
      if (!article_id) {
        this.$router.replace({ path: '/' })
      }
      getShareArticleDetails({ article_id }).then(res => {
        let data = res.data
        data['create_time'] = parseTime(data['create_time'].replaceAll('-', '/'), '{y}/{m}/{d} {h}:{i}')
        this.detail = data
        let wxConfig = {
          title: data.share_title || data.title,
          url: location.href,
          desc: data.desc || '',
          link: window.location.origin + window.location.pathname + '?id=' + data.id,
          imgUrl: data.imgs.length > 0 ? data.imgs[0] : 'http://api.skyorange.cn/logo.jpg'
        }
        setShareInfo(wxConfig)
      })
    }
  }
}
</script>
<style lang="scss">
.detailpage {
  padding: 30px 18px;
  overflow-x: hidden;
  position: relative;
  background: rgba(0, 0, 0, 0);
  box-sizing: border-box;
  z-index: 1;
  //   圆
  &::before {
    content: '';
    border-radius: 50%;
    width: 120px;
    height: 120px;
    overflow: hidden;
    position: absolute;
    top: -60px;
    z-index: -1;
    right: -60px;
    background: radial-gradient(circle, #1ec7a8, #3fe4c6, #9cebdf);
  }
  .title {
    font-size: 18px;
    font-weight: 600;
    color: #555454e0;
    padding: 0 8px;
    box-sizing: border-box;
  }
  .label {
    display: flex;
    justify-content: space-between;
    // border-bottom: 1px solid #d2d2d2;
    align-items: flex-end;
    margin-top: 10px;
    padding: 20px 0;
    margin: 0 8px;
    box-sizing: border-box;
    p {
      font-size: 14px;
      color: #93d7c6;
    }
    .time {
      font-size: 12px;
      color: #d2d2d2;
    }
  }
  .content-text {
    line-height: 2;
    font-size: 16px;
    padding: 20px 0;
    margin: 0 8px;
    color: #949292;
  }
  .content-imglist {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-wrap: wrap;
    padding-left: 8px;

    .imgstyle {
      flex-shrink: 0;
      //   background: #;
      height: 102px;
      width: 102px;
      margin-right: 6px;
      margin-bottom: 6px;
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
  }
  .h-imglist {
    padding: 8px;
    overflow: hidden;
    .h-img {
      width: 100%;
      box-sizing: border-box;
      margin-bottom: 8px;
      img {
        width: 100%;
        object-fit: contain;
      }
    }
  }
  .button {
    position: fixed;
    bottom: 60px;
    right: 23px;
    width: 50px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background: #767676;
    box-shadow: 0 0 8px #767676;
  }
}
</style>
