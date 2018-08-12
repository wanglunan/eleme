<template>
  <div id="app">
    <ele-header :supports="supports" :seller="seller" :description="description" :discountNum="discountNum"></ele-header>
    <div class="index-tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
import header from './components/header/header.vue'

export default {
  name: 'App',
  data () {
    return {
      //获取到的所有商品信息
      seller: {},
      //获取优惠信息数组
      supports: [],
      //获取所有优惠信息
      description: '',
      //获取优惠信息的个数
      discountNum: ''
    }
  },
  created () {
    document.querySelector('html').style.fontSize = 62.5 + '%';
    this.$http.get('./static/data.json').then((res) => {
      this.seller = res.data.seller
      this.supports = res.data.seller.supports
      this.description = this.supports[0].description
      this.discountNum = this.supports.length
      /*console.log(this.seller)
      console.log(this.supports)
      console.log(this.description)*/
    }, (err) => {
      console.log(err)
    })
  },
  components: {
    'ele-header': header
  }
}
</script>

<style>
.clearfix:after { content:""; display: block; clear:both; }
.index-tab{ width: 100%;display: flex;flex-direction: row;height: 50px;border-bottom: 1px solid #eee; }
.tab-item{ flex: 1;font-size: 24px;line-height: 50px;text-align: center }
</style>
