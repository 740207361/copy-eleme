<template>
  <div class="ratings" ref="rating">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star class="star" :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star class="star" :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <slipt></slipt>
      <ratingselect :select-type="selectType" :only-content="onlyContent"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" :key="rating.index" class="rating-item">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar" alt="">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star class="star" :size="24" :score="rating.score"></star>
                <span class="devivery" v-show="rating.deliveryTime">
                  {{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span v-for="item in rating.recommend" :key="item.index">
                </span>
              </div>
              <div class="time">{{rating.rateTime  | formatDate}}</div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import star from '../star/star'
  import slipt from '../slipt/slipt'
  import ratingselect from '../ratingselect/ratingselect'
  import BScroll from 'better-scroll'
  const POSITIVE = 0
  const NEGATIVE = 1
  const ALL = 2
  const ERR_OK = 0
    export default {
      props: {
        seller: {
          type: Object
        }
      },
      data () {
        return {
          ratings: [],
          selectType: ALL,
          onlyContent: true
        }
      },
      created () {
        this.$http.get('/api/ratings').then((res) => {
          res = res.body
          if (res.errno === ERR_OK) {
            this.ratings = res.data
            this.$nextTick(() => {
              if (!this.scroll) {
                this.scroll = new BScroll(this.$refs.rating, {
                  click: true
                })
              } else {
                this.scroll.refresh()
              }
            })
          }
        })
      },
      components: {
        star,
        slipt,
        ratingselect
      },
      filters: {
        formatDate: function (value) {
          let date = new Date(value)
          let y = date.getFullYear()
          let MM = date.getMonth() + 1
          MM = MM < 10 ? ('0' + MM) : MM
          let d = date.getDate()
          d = d < 10 ? ('0' + d) : d
          let h = date.getHours()
          h = h < 10 ? ('0' + h) : h
          let m = date.getMinutes()
          m = m < 10 ? ('0' + m) : m
          let s = date.getSeconds()
          s = s < 10 ? ('0' + s) : s
          return y + '-' + MM + '-' + d + ' ' + h + ':' + m + ':' + s
        }
      }
    }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .ratings
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
      display flex
      padding 18px 0
      .overview-left
        padding 6px 0
        flex 0 0 137px
        width 137px
        border-right 1px solid rgba(7, 17, 27, 0.1)
        text-align center
        @media only screen and (max-width: 320px)
          flex 0 0 120px
          width 120px
        .score
          margin-bottom 6px
          line-height 28px
          font-size 24px
          color rgb(255, 153, 0)
        .title
          margin-bottom 8px
          line-height 12px
          font-size 12px
          color rgb(7, 17, 27)
        .rank
          padding-bottom 6px
          line-height 10px
          font-size 10px
          color rgb(147, 153, 159)
      .overview-right
        flex 1
        padding 6px 0 6px 24px
        @media only screen and (max-width: 320px)
         padding-left 4px
        .score-wrapper
          margin-bottom 8px
          font-size 0
          .title
            display inline-block
            vertical-align top
            line-height 18px
            font-size 12px
            color rgb(7, 17, 27)
          .star
            display inline-block
            margin 0 12px
          .score
            display inline-block
            vertical-align top
            font-size 12px
            color: rgb(255, 153, 0)
        .delivery-wrapper
          font-size 0
          .title
            line-height 18px
            font-size 12px
            color rgb(7, 17, 27)
          .delivery
            margin-left 12px
            font-size 10px
            color rgb(147, 153, 159)
    .rating-wrapper
      padding 0 18px
      .rating-item
        display flex
        padding 18px 0
        .avatar
          flex 0 0 28px
          width 28px
          margin-right 12px
          img
            border-radius 50%
        .content
          position relative
          flex 1
          .name
            margin-bottom 4px
            line-height 12px
            font-size 10px
            color rgb(7, 17, 27)
          .star-wrapper
            margin-bottom 6px
            font-size 0
            .star
              display inline-block
              margin-right 6px
              vertical-align top
            .delivery
              display inline-block
              vertical-align top
          .text
            margin-bottom 8px
            line-height 12px
            color rgb(7, 17, 27)
            font-size 12px
          .recommend
            line-height 16px
            .item
              padding 0 6px
              border 1px solid rgba(7, 17, 27, 0.1)
              border-radius 1px
              color rgb(147, 153, 159)
              background #fff
          .time
            position absolute
            top 0
            right 0
            font-size 10px
            color rgb(147, 153, 159)
</style>
