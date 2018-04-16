<template>
  <cube-page type="scroll-view" title="Scroll" class="cube-page">
    <div slot="content" class="scroll-list-wrap">
      <header><img  :style="headerStyle" src="http://om0jxp12h.bkt.clouddn.com/IMG_3644.PNG"></header>
      <cube-scroll
        ref="scroll"
        :data="[]"
        :listenScroll="true"
        @scroll="onScrollHandle"
        @pulling-down="onPullingDown"
        :options="scrollOptions">
        <img src="http://om0jxp12h.bkt.clouddn.com/IMG_3657.JPG">
        <template slot="pulldown" slot-scope="props">
          <div
              v-if="props.pullDownRefresh"
              class="cube-pulldown-wrapper"
              :style="pullDownStyle">
            <div class="pulldown-content">
              <img src="http://om0jxp12h.bkt.clouddn.com/pulldow-img.jpg">
              <span v-if="props.beforePullDown">{{ pullDownTip }}</span>
              <span v-else>正在更新...</span>
            </div>
          </div>
        </template>
      </cube-scroll>
      <transition name="surprise-page">
        <div v-if="triggerSurprise"
          @click="surpriseHandle"
          class="surprise-page">
          <img src="http://om0jxp12h.bkt.clouddn.com/ad_fullpage.jpg">
        </div>
      </transition>
      <footer><img src="http://om0jxp12h.bkt.clouddn.com/jd_footer"></footer>
    </div>
  </cube-page>
</template>

<script type="text/ecmascript-6">
import CubePage from '../../components/cube-page.vue'

export default {
  data() {
    return {
      scrollOptions: {
        observeDOM: false,
        pullDownRefresh: {
          threshold: 60,
          stop: 40,
          txt: '更新成功'
        }
      },
      pullDownY: 0,
      pullDownStyle: '',
      opacityStyle: '',
      triggerSurpriseFlag: false,
      triggerSurprise: false
    }
  },
  components: {
    CubePage
  },
  computed: {
    pullDownTip() {
      if (this.pullDownY <= 60) {
        return '下拉刷新...'
      } else if (this.pullDownY <= 90) {
        return '继续下拉有惊喜...'
      } else {
        return '松手得惊喜！'
      }
    },
    headerStyle() {
      return `opacity: ${Math.min(1, Math.max(0, 1 - this.pullDownY / 100))}`
    }
  },
  methods: {
    onScrollHandle(pos) {
      this.pullDownY = pos.y
      if (this.pullDownY > 90) this.triggerSurpriseFlag = true
      this.pullDownStyle = `top:${pos.y + 2}px` // Todo: 找到间隙2px的原因
    },
    onPullingDown() {
      if (this.triggerSurpriseFlag) {
        this.triggerSurprise = true
      }
      this.$refs.scroll.forceUpdate()
    },
    surpriseHandle() {
      this.triggerSurpriseFlag = false
      this.triggerSurprise = false
      // go to other page
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
.cube-page
  .content
    margin: 0 !important
    height: 100%
  .surprise-page
    height: 100%
    position absolute
    top: 0
    left: 0
    z-index: 33
    line-height: 0
    img 
      height: 100%
.scroll-list-wrap 
  height: 100%
  border: 1px solid rgba(0, 0, 0, 0.1)
  border-radius: 5px
  transform: rotate(0deg) // fix 子元素超出边框圆角部分不隐藏的问题
  overflow: hidden
  header
  footer
    position: absolute
    left: 0
    z-index: 32
    line-height: 0
  header
    top: 0
  footer
    bottom: -1px
  img
    width: 100%
    // height: 50px
  .cube-pulldown-wrapper
    transform: translateY(-100%)
    line-height: 0
    .pulldown-content
      span
        position: absolute
        bottom: 15px
        left: 0
        right: 0
        margin: auto
        width: 200px
        text-align: center
        color: grey
        font-size: 14px
  .surprise-page-enter, .surprise-page-leave-to
    transform: translateY(-100%)
  .surprise-page-enter-active, .surprise-page-leave-active
    transition: transform .3s
</style>
