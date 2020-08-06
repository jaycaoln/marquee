<template>
  <div class="machine-container">
    <p class="title-text">你已抽取 {{topNum || '-'}} 倍</p>
    <div class="num-box">
      <div class="marquee-double" v-for="(numItem, numIdx) in nums" :key='numIdx'>
        <ul class="marquee-canvas" :class="{[`marquee-animate${numIdx + 1}`]: status === 1, 'finished-animate': status === 2}">
          <li v-if='status === 0' class="marquee-item-question">?</li>
          <li class="marquee-item" v-for="(item, idx) in numItem" :key='idx'>{{ item }}</li>
        </ul>
      </div>
    </div>
    <div @click='handleClick' class="double-btn">点击抽奖</div>
    <p class="residue-times">剩余抽奖次数：{{ times }}</p>
  </div>
</template>

<script>
import {drawCard} from '../mock'
export default {
  data () {
    return {
      nums: [[], [], [], []],
      times: 100,
      topNum: 0,
      status: 0, // 0初始化 1开始 2结束
      disableBtn: false
    }
  },
  methods: {
    handleFrameNum (num) {
      let tempString = String(num)
      if (tempString.length === 3) {
        tempString = '0' + tempString
      }
      const column1 = []
      const column2 = []
      const column4 = []
      const column3 = []
      for (let i = 0; i <= tempString[0]; i++) {
        column1.push(i)
      }
      for (let i = 0; i <= tempString[1]; i++) {
        column2.push(i)
      }
      for (let i = 0; i <= tempString[2]; i++) {
        column3.push(i)
      }
      for (let i = 0; i <= tempString[3]; i++) {
        column4.push(i)
      }
      this.nums = [
        [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...column1],
        [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...column2],
        [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...column3],
        [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, ...column4]
      ]
      this.timer = setTimeout(() => {
        this.status = 2
        this.topNum = num
        this.disableBtn = false
      }, 5000)
    },
    async handleClick () {
      if (this.status === 1 || this.disableBtn) { // 如果正在抽奖
        return
      }
      if (this.times === 0) { // 无抽奖次数
        return
      }
      if (this.timer) { // 清除定时器
        clearTimeout(this.timer)
      }
      this.disableBtn = true
      const data = await drawCard()
      this.status = 1 // 设置为开始抽奖
      this.times -= 1
      this.handleFrameNum(data)
    }
  },
  beforeDestroy() {
    this.timer && clearInterval(this.timer)
  }
}
</script>

<style lang="stylus" scoped>
.machine-container
  .title-text
    font-size 22px
    font-weight 500
    color rgba(255,254,254,1)
    text-align center
  .num-box
    margin-top 38px
    height 110px
    width 500px
    display flex
    background rgba(255,241,201,1)
    .marquee-double
      height 110px
      flex 1
      overflow hidden
      position relative
      .marquee-animate1
        animation marquee-double 5s
      .marquee-animate2
        animation marquee-double 3.5s
      .marquee-animate3
        animation marquee-double 2.8s
      .marquee-animate4
        animation marquee-double 2.5s
      .finished-animate
        transform translateY(calc(-100% + 110px))
      .marquee-canvas
        font-size 60px
        font-weight 400
        color #0F2C55
        width 100%
        text-align center
        animation-fill-mode forwards
        .marquee-item
          height 110px
          line-height 110px
          list-style: none
        .marquee-item-question
          height 110px
          line-height 110px
          list-style: none
  .double-btn
    margin 0 auto
    margin-top 50px
    height 69px
    width 181px
    border 1px solid #ffffff
    padding 10px 20px
    border-radius 4px
    font-weight 800
    font-size 44px
    color white
    &:hover
      cursor pointer
  .residue-times
    font-size 16px
    color #fff
    text-align center
    margin-top 20px
  @keyframes marquee-double{
    from {
      transform translateY(0)
    }
    to {
      transform translateY(calc(-100% + 110px))
    }
  }
</style>
