<template>
   <div class="marquee">
    <ul class="marquee-canvas" :style="styleObject">
      <li class="marquee-item" v-for="(item, index) in data" :key='index'>{{item}}</li>
    </ul>
  </div>
</template>

<script>
/**
 * 跑马灯
 * | 使用的keyframes infinite循环滚动 |
 */
import {fetchMarqueeData} from '../mock/index'
export default {
  data () {
    return {
      data: [],
      styleObject: {}
    }
  },
  methods: {
     scrollInfinite () {
      if (this.data.length > 3) { // 数据 > 3
        var marquees = document.querySelectorAll('.marquee')
        marquees.forEach(function (marquee) {
          var canvas = marquee.querySelector('.marquee-canvas')
          var items = marquee.querySelectorAll('.marquee-item')
          var itemsToClone = Array.from(items).slice(0, 4)
          itemsToClone.forEach(function (item) {
            var clone = item.cloneNode(true)
            canvas.appendChild(clone)
          })
        })
        const firstItem = document.querySelectorAll('.marquee-item')[0].clientWidth
        const secondItem = document.querySelectorAll('.marquee-item')[1].clientWidth
        const thirdItem = document.querySelectorAll('.marquee-item')[2].clientWidth
        const fourthItem = document.querySelectorAll('.marquee-item')[3].clientWidth
        this.styleObject = { // 四个元素宽度 + marginLeft 20px * 4
          '--initX': firstItem + secondItem + thirdItem + fourthItem + 80 + 'px',
          animation: `marquee ${(this.data.length + 4) * 3}s infinite linear`
        }
      } else {
        this.styleObject = {
          width: '100%',
          justifyContent: 'center'
        }
      }
    }
  },
  async created () {
    this.data = await fetchMarqueeData()
    this.$nextTick(() => {
      this.scrollInfinite()
    })
  },
}
</script> 

<style lang='stylus' >
.marquee
  bottom 20px
  height 60px
  width 660px
  overflow hidden
  position relative
  .marquee-canvas
    position absolute
    font-size 12px
    color rgba(255,241,201,1)
    font-weight 500
    display flex
    flex-flow row nowrap
    .marquee-item
      white-space nowrap
      list-style none
      margin-right 20px
    @keyframes marquee {
      from {
        transform translateX(0)
      }
      to {
        transform translateX(calc(-100% + var(--initX)))
      }
    }
</style>
