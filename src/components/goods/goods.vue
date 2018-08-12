<template>
    <div>
        <div class="goods">
            <div class="good-tab">
                <ul>
                    <li :class="index == 0 ? {goodBg: true} : ''" @click="changTab(index)" class="goodsItem" v-for="(good, index) in goodsTitle" :key="index">{{ good }}</li>
                </ul>
            </div>
            <div id="scrollDiv" class="foods-wrapper" ref="foodsWrapper">
                <ul>
                    <li class="food-list-hook" v-for="(good, index) in goods" :key="index">
                        <h1 class="title">{{ good.name }}</h1>
                        <ul>
                            <li class="food-item" v-for="(food, index) in good.foods" :key="index">
                                <div class="food-icon">
                                    <img style="width:57px;height:57px;" :src="food.icon">
                                </div>
                                <div class="food-intro">
                                    <h3>{{ food.name }}</h3>
                                    <p>{{ food.description }}</p>
                                    <p>月售<span>{{ food.sellCount }}</span>份 好评率<span>{{ food.rating }}</span>%</p>
                                    <p class="price">￥{{ food.price }}</p>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <cartcontrol :food="food" :update-food-count="updateFoodCount"></cartcontrol>
                                </div>
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>
            <!--shopcart :food-list="foodList"
                :delivery-price="seller.deliveryPrice"
                :min-price="seller.minPrice"
                :clear-cart="clearCart"
                :update-food-count="updateFoodCount"
                ref="shopcart"></shopcart-->
            <shopcart :clear-cart="clearCart" :min-price="minPrice" :delivery-price="deliveryPrice" :food-list="foodList" :update-food-count="updateFoodCount" ref="shopcart"></shopcart>
        </div>
    </div>
</template>
<script>
import Vue from 'vue'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import shopcart from '../shopcart/shopcart.vue'


export default {
    props: ['seller'],
    data () {
        return{
            //商品分类数据
            goods: [],
            //商品分类title
            goodsTitle: [],
            //商品详细信息数组
            foods: [],
            //获取右侧商品距顶部的TOP值数组
            foodTop: [],
            //配送费
            deliveryPrice: 0,
            //起送费
            minPrice: 0
        }
    },
    created () {
        this.$http.get('./static/data.json').then((res) => {
            this.deliveryPrice = res.data.seller.deliveryPrice
            this.minPrice = res.data.seller.minPrice
            this.goods = res.data.goods
            var num = this.goods.length
            for( let i = 0;i < num;i++ ){
                this.goodsTitle.push( this.goods[i].name ) 
                this.foods.push(this.goods[i].foods)
            }
            //console.log(this.foods)
            this.$nextTick(()=>{
                this._initTops ()
            })
        }, (err) => {
            console.log(err)
        });
    },
    mounted () {
        //监听滚动函数的时候第三个参数必须带上true,且监听只能在mounted里监听
        window.addEventListener('scroll', this._scroll, true);
    },
    methods: {
        //获取右侧商品距顶部的TOP值数组
        _initTops () {
            var lis = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
            //console.log(lis);
            for( let i = 0;i < lis.length;i++ ){
                this.foodTop.push(lis[i].offsetTop);
            }
            //console.log(this.foodTop)
        },
        //左侧列表点击事件
        changTab (index) {
            $('.foods-wrapper').animate({
                scrollTop: this.foodTop[index] + 'px'
            });
            $('.goodsItem').eq(index).addClass('goodBg').siblings().removeClass('goodBg');
        },
        //监听滚动事件
        _scroll () {
            var scrollDiv = document.getElementById('scrollDiv');
            //获取滚动条滚动的高度
            var _top = scrollDiv.scrollTop;
            var scrollFoodTop = [];
            //取出各li距顶部的距离
            scrollFoodTop.push(this.foodTop);
            //console.log(scrollFoodTop)
            for( let i = 0;i < scrollFoodTop[0].length;i++ ){
                (function(i){
                    //28是每种食品的title的高度
                    if( _top >= scrollFoodTop[0][i] - 28 ){
                        //console.log(scrollFoodTop[0][i])
                        $('.goodsItem').eq(i).addClass('goodBg').siblings().removeClass('goodBg');
                    }
                })(i)
            }
        },
        //加减号组件函数
        updateFoodCount (food, isAdd) {
            if(isAdd){
                //如果count属性不存在，vue.set设置该属性值为1
                if(!food.count){
                    Vue.set(food, 'count', 1)
                }else{
                    food.count++
                }
                // 通知shopcart组件对象启动一个小球的显示动画
                this.$refs.shopcart.drop(event.target)
            }else{
                if(food.count){
                    food.count--
                }
            }
        },
        //清空购物车函数
        clearCart () {
            //不能用for循环遍历，否则点击清空的时候购物车列表项会出现逐个消失的情况
            this.foodList.map(function(item){
                return item.count = 0
            })
        }
    },
    computed: {
        //给shopcart组件传所有带有food.count属性的数据
        foodList () {
            //不能用data中的foods数据，加减号函数修改的是接收数据中的count属性
            const foods = [];
            this.goods.forEach(good => {
                good.foods.forEach(food => {
                    //查找出所有带有count属性的food,然后传入shopcart组件中
                    if(food.count) {
                        foods.push(food)
                    }
                })
            })
            return foods
        }
    },
    components: {
        cartcontrol: cartcontrol,
        shopcart: shopcart
    }
}
</script>
<style scoped>
    .goods{ display: flex;position: absolute;top: 320px;left: 0;bottom: 46px;width: 100%; }
    .good-tab{ flex: 0 0 100px;width: 100px;background: #f3f5f7; }
    .goodBg{ background-color: #fff; }
    .goodsItem{ font-size: 14px;border-bottom: 1px solid #eee;padding: 15px 10px;width: 80px;text-align: center }
    .foods-wrapper{ overflow-y: auto; }
    .title{ background-color: #f3f5f7;font-size: 16px;border-left: 2px solid #eee;text-indent: 20px;color: rbg(240,20,20);line-height: 28px; }
    .food-item{ background-color: #fff;display: flex;padding: 15px 0;flex-direction: row;border-bottom: 1px solid #f3f5f7;position: relative; }
    .food-icon{ flex: 1;margin: 0 20px; }
    .food-intro{ flex: 5;margin: 0; }
    .food-intro h3{ font-size: 18px;color: #000;margin: 0; }
    .food-intro p{ margin-top: 5px;color: rbg(147, 152, 159); }
    .price{ font-size: 16px;color: red;font-weight: bold }
    .cartcontrol-wrapper{ position: absolute;right: 16px;bottom: 10px; }
</style>


