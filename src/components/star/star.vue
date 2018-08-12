<template>
    <div class="star">
        <span class="star-item" :class="c" v-for="(c, index) in starClasses" :key="index"></span>
    </div>
</template>
<script>
  /*
  最高分: 5
  小数部分>=0.5
   */
  const CLASS_ON = 'on'
  const CLASS_HALF = 'half'
  const CLASS_OFF = 'off'

  export default {
    props: {
      score: Number
    },
    computed: {
      starClasses () {
        const scs = []
        // 向scs添加CLASS_ON
        var score = this.score
        var scoreInteger = Math.floor(score)
        for (let i = 0; i < scoreInteger; i++) {
          scs.push(CLASS_ON)
        }
        // 向scs添加CLASS_HALF
        if(score-scoreInteger>=0.5) {
          scs.push(CLASS_HALF)
        }
        // 向scs添加CLASS_OFF
        while(scs.length<5) {
          scs.push(CLASS_OFF)
        }
        return scs
      }
    }
  }
</script>
<style scoped>
    .star{ margin: 20px auto; }
    .star-item{ display: inline-block;width: 40px;height: 40px;margin: 0 5px;background-size: 100% 100%;background-repeat: no-repeat; }
    .on{ background: url(star48_on@2x.png); }
    .half{ background: url(star48_half@2x.png); }
    .off{ background: url(star48_off@2x.png); }
</style>

