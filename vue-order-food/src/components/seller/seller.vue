<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">{{seller.sellCount}}</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>分钟
            </div>
          </li>
        </ul>
      </div>
      <slipt></slipt>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item" v-for="(item, index) in seller.supports" :key="item.index">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <slipt></slipt>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="pic in seller.pics" :key="pic.index">
              <img :src="pic" width="120" height="90" alt="">
            </li>
          </ul>
        </div>
      </div>
      <slipt></slipt>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos" :key="info.index">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import star from '../star/star'
  import slipt from '../slipt/slipt'
  import BScroll from 'better-scroll'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    components: {
      star,
      slipt
    },
    methods: {
      _initScroll () {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      },
      _initPics () {
        if (this.seller.pics) {
          let picWidth = 120
          let margin = 6
          let width = (picWidth + margin) * this.seller.pics.length
          this.$refs.picList.style.width = width
          this.$nextTick(() => {
            if (!this.picscroll) {
              this.picscroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true
              })
            } else {
              this.picscroll.refresh()
            }
          })
        }
      }
    },
    watch: {
      seller () {
        this._initScroll()
        this._initPics()
      }
    },
    mounted () {
      this._initScroll()
      this._initPics()
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .seller
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
      padding 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7, 17, 27)
        font-size 14px
      .desc
        padding-bottom 18px
        line-height 18px
        font-size 0
        .star
          display inline-block
          margin-right 8px
          vertical-align top
        .text
          display inline-block
          margin-right 12px
          vertical-align top
          color rgb(77, 85, 93)
          font-size 10px
      .remark
        display flex
        padding-top 18px
        .block
          flex 1
          text-align center
          border-right 1px solid rgba(7, 17, 27, 0.1)
          &.last-child
            border none
          h2
            margin-bottom 4px
            line-height 10px
            font-size 10px
          .content
            line-height 24px
            font-size 10px
            color rgb(7, 17, 27)
            .stress
              font-size 24px
    .bulletin
      padding 18px 18px 0 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7, 17, 27)
        font-size 14px
      .content-wrapper
        padding 0 12px 16px 12px
        .content
          line-height 24px
          font-size 12px
          color rgb(240, 20, 20)
      .supports
        .support-item
          padding 16px 12px
          font-size 0
          .icon
            vertical-align bottom
            display inline-block
            width 12px
            height 12px
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              background-image url("./decrease_4@2x.png")
            &.discount
              background-image url("./discount_4@2x.png")
            &.guarantee
              background-image url("./guarantee_4@2x.png")
            &.invoice
              background-image url("./invoice_4@2x.png")
            &.special
              background-image url("./special_4@2x.png")
          .text
            line-height 16px
            font-size 12px
    .pics
      padding 18px
      .title
        margin-bottom 12px
        line-height 14px
        color rgb(7, 17, 27)
        font-size 14px
      .pic-wrapper
        width 100%
        overflow hidden
        white-space nowrap
        .pic-list
          font-size 0
          .pic-item
            display inline-block
            margin-right 6px
            width 120px
            height 90px
    .info
      padding 18px 18px 0 18px
      .title
        padding-bottom 12px
        line-height 14px
        color rgb(7, 17, 27)
        font-size 14px
      .info-item
        padding 16px 12px
        line-height 16px
        font-size 12px
        color rgb(7, 17, 27)
</style>
