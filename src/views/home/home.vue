<template>
  <div class="homepage">
    <Header />
    <!-- 头部 -->
    <div class="header" id="tabTop" v-if="listTopobj.cover">
      <img :src="listTopobj.cover" alt="" />
      <!-- 个人信息 -->
      <div class="user-info">
        <div class="user-name">{{ listTopobj.title }}</div>
      </div>
    </div>
    <!-- 内容 -->
    <div class="main">
      <!-- <div class="main-header">
        <ul class="flexbetween">
          <li @click="onDk" :style="is_dk && 'color:#3fe4c6'">
            <i class="iconfont icon-sign icon-size"></i>
            <p>{{ is_dk ? '已打卡' : '打卡' }}</p>
          </li>
          <li @click="goImagepage">
            <i class="iconfont icon-tuku icon-size"></i>
            <p>图库</p>
          </li>
          <li @click="goAbout">
            <i class="iconfont icon-hezuo icon-size"></i>
            <p>合作</p>
          </li>
          <li @click="onshare">
            <i class="iconfont icon-fenxiang icon-size"></i>
            <p>分享</p>
          </li>
        </ul>
      </div> -->
      <div class="main-content">
        <van-sticky>
          <div class="main-content-tab">
            <van-tabs
              v-model="listQuery.label"
              :title-inactive-color="titleinactivecolor"
              :title-active-color="titleactivecolor"
              :color="titleactivecolor"
              :line-width="linewidth"
              @change="tabchange"
              title-class="tabStyle"
            >
              <van-tab v-for="(item, index) in typelist" :name="item.id" :title="item.name"> </van-tab>
            </van-tabs>
          </div>
        </van-sticky>
        <!-- 列表 -->
        <div class="man-list">
          <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="getList">
            <div v-for="(item, index) in list" :key="index" class="block" @click="goDetail(item.id)">
              <div class="left">
                <img :src="item.cover" alt="" />
              </div>
              <div class="right">
                <div class="title">{{ item.title }}</div>
                <div class="tag">
                  <div class="time">{{ item.create_time }}</div>
                  <div class="time">{{ item.label }}</div>
                </div>
              </div>
            </div>
          </van-list>
        </div>
      </div>
    </div>
    <div class="page-tag">天橙浏览器</div>
    <!-- 分享提示 -->
    <van-popup
      v-model="isshareShow"
      :close-on-popstate="true"
      position="top"
      :style="{ maxHeight: '100%', background: 'rgba(0,0,0,0)' }"
    >
      <img
        style="width: 100%; box-sizing: border-box; padding: 20px 20px 56px 144px"
        src="@/assets/fx.png"
        alt=""
        @click="
          () => {
            isshareShow = false
          }
        "
      />
    </van-popup>
  </div>
</template>

<script>
import Header from '@/components/Header'

import { getShareArticleList } from '@/api/user'
import { formatTime } from '@/utils'
import { setShareInfo } from '@/utils/share'

export default {
  components: {
    Header
  },
  data() {
    return {
      is_dk: false,
      titleinactivecolor: '#ccc',
      titleactivecolor: '#333',
      linewidth: '20px',
      list: [],
      listTopobj: null,
      iszan: false,
      isshareShow: false,
      loading: false,
      finished: false,
      listQuery: {
        label: '',
        page: 1
      },
      zanlist: [],

      typelist: [
        {
          id: '',
          name: '全部'
        },
        {
          id: '空间',
          name: '空间'
        },
        {
          id: '建筑',
          name: '建筑'
        },
        {
          id: '景观',
          name: '景观'
        },
        {
          id: '设计',
          name: '设计'
        },
        {
          id: '项目',
          name: '项目'
        },
        {
          id: '资讯',
          name: '资讯'
        },
        {
          id: '人物',
          name: '人物'
        },
        {
          id: '深度',
          name: '深度'
        }
      ]
    }
  },
  created() {
    let wxConfig = {
      title: '天空之橙·Design｜建筑·空间·景观·运营',
      url: location.href,
      desc: '',
      //   link: location.href,
      link: window.location.href,

      imgUrl: 'http://api.skyorange.cn/logo.jpg'
    }
    setShareInfo(wxConfig)
    this.getList()
  },
  methods: {
    goAbout() {
      this.$router.push({ path: '/about' })
    },
    goImagepage() {
      this.$router.push({ path: '/imagelist' })
    },
    scrollIntoView(id) {
      console.log(this.$el.querySelector(id))
      this.$el.querySelector(id).scrollIntoView({
        behavior: 'smooth', // 平滑过渡
        block: 'start' // 上边框与视窗顶部平齐。默认值
      })
    },
    stopClick(e) {
      console.log(e)
    },
    zanClick(id) {
      if (this.zanlist.includes(id)) {
        this.zanlist = this.zanlist.filter(item => item != id)
      } else {
        this.zanlist.push(id)
      }
    },
    tabchange(name, title) {
      this.listQuery.page = 1
      this.finished = false
      console.log(name, title)
      //   this.scrollIntoView('#tabTop')
      this.getList()
    },
    onDk() {
      if (!this.is_dk) {
        this.is_dk = true
        this.$toast.success('打卡成功')
      }
    },
    onshare() {
      this.isshareShow = true
    },
    getList() {
      let { page, label } = this.listQuery
      this.loading = true
      getShareArticleList({ page, label }).then(res => {
        res.data = res.data.map(item => {
          item['create_time'] = formatTime(+new Date(item['create_time'].replaceAll('-', '/')), '{y}-{m}-{d} {h}:{i}')
          return item
        })

        if (this.listQuery.page == 1) {
          this.list = res.data
        } else {
          this.list = this.list.concat(res.data)
        }
        if (this.listQuery.label === '') {
          this.listTopobj = this.list[0]
        }
        // 加载状态结束
        this.loading = false
        this.listQuery.page++
        // 数据全部加载完成
        if (res.data.length <= 0) {
          this.finished = true
        }
      })
    },
    goDetail(data) {
      console.log(data)
      this.$router.push({
        path: '/detail',
        query: {
          id: data
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.colorRed {
  color: #fc5531;
  font-size: 24px;
}
.homepage {
  position: relative;
  background: rgba(0, 0, 0, 0);
  background: #fff;
  overflow: hidden;
  z-index: 1;
  padding-top: 60px;
  //   &::before {
  //     content: '';
  //     position: absolute;
  //     top: 0;
  //     width: 100%;
  //     height: 320px;
  //     background: #ccc url('../../assets/bg.jpg') no-repeat;
  //     background-size: 100% cover;
  //     z-index: -1;
  //     // animation: fadenum12 30s ;
  //     animation-name: fadenum12;
  //     animation-timing-function: ease-in-out;
  //     animation-iteration-count: infinite;
  //     animation-duration: 50s;
  //   }
  //   background-size: cover;
  .header {
    background: rgba(0, 0, 0, 0);
    height: 210px;
    display: flex;
    justify-content: flex-start;
    align-items: flex-end;
    box-sizing: border-box;
    position: relative;
    z-index: 1;
    img {
      z-index: -1;
      position: absolute;
      top: 0;
      width: 100%;
      height: 100%;
      display: block;
      object-fit: cover;
    }
    .user-info {
      color: #fff;
      background: rgba(0, 0, 0, 0.4);
      padding: 10px 18px;
      width: 100%;
      box-sizing: border-box;
      .user-name {
        font-size: 16px;
        @include textoverflow(2);
      }
      .user-desc {
        font-size: 12px;
        @include textoverflow(2);
      }
    }
  }

  .main {
    .main-content {
      background: #fff;
      .main-content-tab {
        .van-tabs {
          .van-tabs__line {
            bottom: 0.5rem;
          }
          .van-tab {
            font-weight: 700;
          }
        }
      }
      .man-list {
        .block {
          height: 100px;
          width: 100%;
          box-sizing: border-box;
          border-bottom: 1px solid #f7f7f7;
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 10px 10px;
        }
        .left {
          width: 34%;
          height: 100%;
        }

        .left img {
          width: 100%;
          height: 100%;
          object-fit: cover;
        }

        .right {
          flex: 1;
          height: 100%;
          padding-left: 8px;
          box-sizing: border-box;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
        }

        .title {
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 2;
          line-height: 1.5;
          overflow: hidden;
          font-size: 14px;
        }

        .time {
          color: #9b9b9b;
          font-size: 12px;
        }
        .tag {
          color: #9b9b9b;
          font-size: 12px;
          display: flex;
          justify-content: space-between;
          align-items: center;
        }
      }
    }
  }
  .page-tag {
    font-size: 12px;
    color: #cecece;
    text-align: center;
    padding: 10px 0;
  }
}
</style>
