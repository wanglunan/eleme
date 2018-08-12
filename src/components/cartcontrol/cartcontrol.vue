<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease" v-show="food.count" @click.stop="updateFoodCount(food, false)">-</div>
        </transition>
        <div class="cart-count" v-show="food.count">{{ food.count }}</div>
        <div class="cart-add" @click.stop="updateFoodCount(food, true)">+</div>
    </div>
</template>
<script>
export default {
    props: {
        food: Object,
        updateFoodCount: Function
    }
}
</script>
<style scoped>
    .cart-decrease{ cursor: pointer;margin: 0 5px;display: inline-block;width: 26px;height: 26px;color: #fff;font-size: 20px;color: #fff;background-color: rgb(0, 160, 220);line-height: 22px;text-align: center;font-weight: bold;border-radius: 50%; }
    .cart-add{ cursor: pointer;margin: 0 5px;display: inline-block;width: 26px;height: 26px;color: #fff;font-size: 20px;color: #fff;background-color: rgb(0, 160, 220);line-height: 22px;text-align: center;font-weight: bold;border-radius: 50%; }
    .cartcontrol{ display: inline-block; }
    .cart-count{ display: inline-block;font-size: 16px;color: rbg(147, 153, 159);line-height: 24px;text-align: center }
    .move-enter-active, .move-leave-active{ transition: all .4s linear }
    .move-enter, .move-leave-active{ opacity: 0;transform: translate3d(24px, 0, 0) rotate(180deg) }
</style>


