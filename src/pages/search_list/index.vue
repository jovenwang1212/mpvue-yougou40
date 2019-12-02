<template>
  <div>
    <div class="header">
      <icon type="search" size="16" color="#bdbdbd"> </icon>
      <input type="text" v-model="query" @confirm="reload" confirm-type="search"/>
    </div>

    <!-- 过滤栏 -->
    <ul class="filter-menu">
      <li
        v-for="(item, index) in menuList"
        :key="index"
        :class="{active:acitveIndex === index}"
        @click="acitveIndex = index"
      >
        {{ item }}
      </li>
    </ul>

    <ul class="goods-list">
      <li class="goods" v-for="(item, index) in goodsList" :key="index">
        <img
          :src="item.goods_small_logo"
          alt=""
        />
        <div class="right">
          <p class="text-line2">
            {{item.goods_name}}
          </p>
          <span class="price">￥<span class="num">{{item.goods_price}}</span>.00</span>
        </div>
      </li>
    </ul>
    <div class="hasBtm" v-show="isLastPage">--我是有底线的--</div>
  </div>
</template>

<script>
const PAGE_SIZE = 6
export default {
  data () {
    return {
      menuList: ['综合', '销量', '价格'],
      acitveIndex: 0,
      query: '',
      pageNum: 1,
      goodsList: [],
      // 是否在请求中，如果在请求中，就不发请求
      isRequest: false,
      isLastPage: false
    }
  },
  onPullDownRefresh () {
  //  加载第一页
    this.reload()
  },
  // 上拉加载下一页
  onReachBottom () {
    this.pageNum++
    this.queryGoodsList()
  },
  onLoad (options) {
    this.query = options.query
    // 查询商品列表
    this.queryGoodsList()
  },
  methods: {
    reload () {
      //  加载第一页
      this.pageNum = 1
      this.goodsList = []
      this.queryGoodsList()
    },
    queryGoodsList () {
      // 如果请求中，就不再请求
      if (this.isRequest || this.isLastPage) {
        return
      }

      this.isRequest = true
      this.$request({
        url: '/api/public/v1/goods/search',
        data: {
          query: this.query,
          pagenum: this.pageNum,
          pagesize: PAGE_SIZE
        }
      }).then(data => {
        console.log(data)
        this.goodsList = [...this.goodsList, ...data.goods]
        // 判断是否到了最后一页
        if (this.goodsList.length === data.total) {
          this.isLastPage = true
        }
      }).finally(() => {
        // 请求完成后，允许再请求了
        this.isRequest = false
      })
    }
  }
}
</script>

<style lang="less">
.header {
  height: 120rpx;
  padding: 0 16rpx;
  display: flex;
  align-items: center;
  background-color: #eee;
  position: relative;
  input {
    height: 60rpx;
    flex: 1;
    border-radius: 6rpx;
    background-color: #fff;
    padding-left: 78rpx;
  }
  icon {
    position: absolute;
    left: 44rpx;
  }
}

.filter-menu {
  display: flex;
  justify-content: space-around;
  height: 100rpx;
  align-items: center;
  .active {
    color: #eb4450;
  }
}
.goods {
  height: 260rpx;
  border-top: 1px solid #ddd;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  padding: 0 20rpx;
  img {
    width: 200rpx;
    height: 200rpx;
  }
  .right {
    flex: 1;
    margin-left: 24rpx;
    p {
      margin-bottom: 70rpx;
    }
    .price {
      color: #eb4450;
      font-size: 24rpx;
      .num {
        font-size: 36rpx;
      }
    }
  }
}
.hasBtm{
  text-align: center;
  color:#666;
}
</style>
