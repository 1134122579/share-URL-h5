<template>
  <div class="typepage">
    <Header />
    <div class="inputstyle">
      <div class="logo">
        <img src="../../assets/yglogo.png" alt="" />
      </div>
      <van-field v-model.trim="value" center clearable placeholder="请输入 建筑 设计 等">
        <template #button>
          <van-button size="small" type="primary" @click="onshare" color="#4e6ef2" block>有光 搜索</van-button>
        </template>
      </van-field>
    </div>
    <!-- 内容 -->
    <div class="main">
      <div class="main-content">
        <div class="man-header">网址导航 Navigation</div>
        <!-- 列表 -->
        <div class="man-list">
          <!-- <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="getList"> -->
          <div v-for="(item, index) in listold" :key="index" class="block" @click="goDetail(item)">
            <div class="right">
              <div class="title"><span class="iconfont icon-icon-"></span> {{ item.name }}</div>
            </div>
          </div>
          <!-- </van-list> -->
        </div>
      </div>
    </div>
    <div class="bottom">
      <div class="nametext">天空之橙·有光</div>
      <div class="text">
        <p>分享传播优秀设计内容</p>
        <p>有趣的灵魂在这里遇见</p>
      </div>
    </div>
    <div class="page-tag">有光·搜索·浏览器1.0</div>
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
      value: '',
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
      listold: [
        {
          name: 'ArchDaily-传播世界建筑',
          url: 'https://www.archdaily.cn/cn?ad_source=jv-header'
        },
        // {
        //   name: '知末案例-全球室内设计案例',
        //   url: 'https://news.znztv.com/classify/0_0_1_0_0_0_0_0_0'
        // },

        {
          name: '搜建筑-关注全球时尚的、新的建筑资讯',
          url: 'https://www.soujianzhu.cn/NewProject/ProjectList.aspx'
        },

        {
          name: '建筑学院-为建筑师而打造的高品质平台',
          url: 'http://www.archcollege.com/'
        },
        {
          name: '设计宇宙-推动高品质项目和机构的分享平台',
          url: ' https://www.designverse.com.cn/'
        },
        // {
        //   name: 'Hi设计-创新商业空间和创新商业IP的数字媒体',
        //   url: 'https://www.hisheji.com/'
        // },
        {
          name: '有方建筑-高品质建筑资讯门户',
          url: 'http://www.archiposition.com/'
        },
        {
          name: '一起设计-设计人在线读物、专业制造设计灵感',
          url: 'https://www.yiqisheji.com/'
        },
        {
          name: '谷德设计网-服务全球创意，推进全球创造',
          url: 'https://www.gooood.cn/'
        }
        // {
        //   name: '专筑网-为设计先锋力量提供的交流平台',
        //   url: 'http://www.iarch.cn/'
        // }
      ],
      listQuery: {
        label: '',
        page: 1
      },
      zanlist: []
    }
  },
  created() {
    this.listold = this.listold.sort((a, b) => {
      return a.name.length - b.name.length
    })
    let wxConfig = {
      title: '天空之橙·有光 | 导航',
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
    onshare() {
      console.log(this.value, 'this.value')
      if (!this.value) {
        this.$toast('请输入内容')
        return
      }
      window.open(`https://www.baidu.com/s?word=${this.value}`)
    },
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
      //   console.log(data)
      //   window.open(data.url)
      //   return
      this.$router.push({
        path: '/typelook',
        query: {
          urltext: data.url
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
.typepage {
  position: relative;
  background: rgba(0, 0, 0, 0);
  overflow: hidden;
  z-index: 1;
  padding-top: 60px;
  .inputstyle {
    width: 100%;
    box-sizing: border-box;
    margin: 20px 0 10px;
    padding: 18px;
    .logo {
      width: 95px;
      margin: 0 auto;
      margin-bottom: 28px;
      img {
        width: 100%;
      }
    }
  }

  .main {
    .man-header {
      width: 100%;
      box-sizing: border-box;
      border-bottom: 1px solid #f7f7f7;
      padding: 10px 10px;
      font-size: 16px;
      color: #4a4a4a;
      //   text-indent: 8px;
      font-weight: 700;
    }
    .main-content {
      background: #fff;
      .man-list {
        .block {
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
  .bottom {
    margin-top: 20px;
    padding: 0 18px;
    font-size: 12px;
    color: #9b9b9b;
    display: flex;
    justify-content: center;
    align-items: center;
    line-height: 1.5;
    .nametext {
      border-right: 1px solid #9b9b9b;
      padding-right: 6px;
      margin-right: 6px;
    }
  }
  .page-tag {
    font-size: 12px;
    transform: scale(0.9);
    color: #cecece;
    text-align: center;
    padding: 8px 0;
  }
}
</style>
