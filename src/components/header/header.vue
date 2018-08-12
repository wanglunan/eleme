<template>
    <div class="header1 clearfix">
        <div class="header-content">
            <img :src="seller.avatar" alt="">
            <div class="header-right">
                <h3><span>品牌</span> {{ seller.name }}</h3>
                <p>{{ seller.description }} / {{ seller.deliveryTime }}分钟送达</p>
                <p><!--span :class=""></span--> {{ description }}</p>
            </div>
        </div>
        <div class="header-more">
            <p @click="showDetail(true)">{{ discountNum }}个</p>
        </div>
        <div class="header-notice">
            <p>{{ seller.bulletin }}</p>
        </div>
        <div class="detail" v-show="isshow">
            <div class="detail-title">
                <h3>{{ seller.name }}</h3>
                <div class="star-wrapper">
                    <star :score="seller.score"></star>
                </div>
            </div>
            <div class="title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
            </div>
            <ul class="supports">
                <li class="support" v-for="(item, index) in supports" :key="index">
                    <span class="icon" :class="classMap[item.type]"></span>
                    <span class="text">{{ item.description }}</span>
                </li>
            </ul>
            <div class="title">
                <div class="line"></div>
                <div class="text">商家公告</div>
                <div class="line"></div>
            </div>
            <div class="content">
                <p>{{ seller.bulletin }}</p>
            </div>
            <div class="detail-close">
                <span class="icon-close" @click="showDetail(false)">X</span>
            </div>
        </div>
    </div>
</template>

<script>
import star from '../star/star.vue'
export default {
  props: ['seller', 'description', 'discountNum', 'supports'],
  data () {
    return {
        //样式数组，用于优惠信息前面的图标
        classMap: ['decrease', 'discount', 'guarantee', 'invoice', 'special'],
        //控制浮窗显示隐藏
        isshow: false
    }
  },
  methods: {
      showDetail: function(isshow){
          this.isshow = isshow
      }
  },
  components: {
    'star': star
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header1{ width: 100%;position: relative;height: 268px;background:rgba(7,17,27,0.5);color: #fff;overflow: hidden; }
.header-content{ display: flex;width: 100%;margin: 0 auto;align-items: center;flex-direction: row; }
.header1 img{ width: 128px;height: 128px;margin: 48px 20px 48px 28px;border-radius: 5px;display: block }
.header-right{ flex: 5; }
.header-right h3{ margin: 0px 0 10px;font-weight: bold;font-size: 24px; }
.header-right h3 span{ display: inline-block;font-size: 18px;text-align: center;padding: 2px 5px;background-color: red; }
.header-right p{ margin-bottom: 10px;font-size: 16px; }
.header-notice{ position: absolute;left: 0;bottom: 0;height: 56px;line-height: 56px;z-index: 9;background-color: #000;font-size: 18px;opacity: 0.8;padding: 0 10px;width: 100%; }
.header-notice p{ overflow: hidden;text-overflow:ellipsis;white-space: nowrap; }
.header-more{ position: absolute;right: 10px;bottom: 100px;text-align: center;width: 50px;height: 40px;line-height: 40px;z-index: 9;border-radius: 15px;font-size: 18px;background-color: #000;opacity: 0.8; }

.detail{ position: fixed;width: 100%;height: 100%;z-index: 999;background-color: #000;opacity: 0.8;left: 0;top: 0; }
.detail-title{ margin-top: 100px; }
.detail-title h3{ font-size: 24px;text-align: center;color: #fff;font-weight: bold; }
.star-wrapper{ text-align: center }
.title{ padding: 28px 36px 24px;display: flex; }
.line{ position: relative;top: 8px;flex: 1;width: 107px;height: 2px;background: rgba(255,255,255,0.2); }
.text{ margin: 0 12px;font-size: 18px;color: #fff; }
.supports{ padding: 0 48px; }
.support{ margin-bottom: 12px;text-align: left;font-size: 18px; }
.icon{ display: inline-block;width: 24px;height: 24px;background-size: 16px 16px;background-repeat: no-repeat;vertical-align: middle; }
.decrease{ background: url(decrease_2@2x.png);background-size: 100% 100%; }
.discount{ background: url(discount_2@2x.png);background-size: 100% 100%; }       
.guarantee{ background: url(guarantee_2@2x.png);background-size: 100% 100%; }    
.invoice{ background: url(guarantee_2@2x.png);background-size: 100% 100%; }
.special{ background: url(special_2@2x.png);background-size: 100% 100%; }
.content{ margin: 0 auto;font-size: 20px;font-weight: 200;line-height: 32px;width: 80%;color: #fff; }
.detail-close{ text-align: center;line-height: 32px;font-size: 32px;height: 32px;margin-top: 160px;color: rgba(255,255,255,0.5);border-radius: 50%; }
</style>
