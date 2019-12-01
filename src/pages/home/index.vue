<template>
  <div>
    <div class="header">
      <div class="inner">
        <icon type="search" size="16" color="#bbb"> </icon>
        <span>搜索</span>
      </div>
    </div>
    <!-- 轮播图 -->
    <swiper
      indicator-dots
      autoplay
      circular
      indicator-color="rgba(255,255,255,0.3)"
      indicator-active-color="#fff"
    >
      <block v-for="(item, index) in swiperdata" :key="index">
        <swiper-item>
          <image :src="item.image_src"></image>
        </swiper-item>
      </block>
    </swiper>

    <!-- 分类 -->
    <ul class="cates">
      <li v-for="(item, index) in catitems" :key="index">
        <img :src="item.image_src" alt="">
      </li>
    </ul>

    <!-- 楼层 -->
    <ul>
      <li class="floor-item" v-for="(item2, index2) in floordata" :key="index2">
        <img src="" alt="">
        <div class="products">
          <img src="https://api.zbztb.cn/pyg/pic_floor01_1@2x.png" alt="">
          <div class="right">
            <img v-for="(item, index) in 4" :key="index" src="https://api.zbztb.cn/pyg/pic_floor01_1@2x.png" alt="">
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      swiperdata: [],
      catitems: [],
      floordata: []
    }
  },
  created () {
    this.getSwiperdata()
    this.getCatitems()
    this.getFloordata()
  },
  methods: {
    getSwiperdata () {
      wx.request({
        url: 'https://ugo.botue.com/api/public/v1/home/swiperdata',
        success: res => {
          console.log(res)
          if (res.data.meta.status === 200) {
            this.swiperdata = res.data.message
          }
        }
      })
    },
    getCatitems () {
      wx.request({
        url: 'https://ugo.botue.com/api/public/v1/home/catitems',
        success: res => {
          console.log(res)
          if (res.data.meta.status === 200) {
            this.catitems = res.data.message
          }
        }
      })
    },
    getFloordata () {
      wx.request({
        url: 'https://ugo.botue.com/api/public/v1/home/floordata',
        success: res => {
          console.log(res)
          if (res.data.meta.status === 200) {
            this.floordata = res.data.message
          }
        }
      })
    }
  }
}
</script>

<style lang="less">
.header {
  height: 100rpx;
  background-color: #eb4450;
  display: flex;
  align-items: center;
  padding: 0 16rpx;
  .inner {
    flex: 1;
    height: 60rpx;
    background-color: #fff;
    border-radius: 8rpx;
    display: flex;
    justify-content: center;
    align-items: center;
    span {
      color: #bbb;
    }
    icon {
      margin-right: 16rpx;
      margin-top: 10rpx;
    }
  }
}

swiper image {
  width: 100%;
  height: 340rpx;
}

.cates{
  height: 194rpx;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  img{
    width: 128rpx;
    height: 140rpx;
  }
}

.floor-item{
  margin-bottom:20rpx;
  >img{
    width: 100%;
    height: 88rpx;
  }
  .products{
    display: flex;
    margin-top:20rpx;
    padding-left:18rpx;
    >img{
      width: 232rpx;
      height: 386rpx;
    }
    .right{
      flex:1;
      font-size: 0;
      >img{
        width: 232rpx;
        height: 188rpx;
        margin-left:10rpx;
        margin-bottom:10rpx;
      }
    }
  }
}
</style>
