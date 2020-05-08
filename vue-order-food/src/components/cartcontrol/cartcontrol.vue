<template>
  <div class="cartcontrol">
    <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart($event)"
    transition="move"><span class="inner">-</span></div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart($event)"><span>+</span></div>
  </div>
</template>

<script>
    import Vue from 'vue'
    export default {
      props: {
        food: {
          type: Object
        }
      },
      methods: {
        addCart (event) {
          if (!event._constructed) {
            return
          }
          if (!this.food.count) {
            Vue.set(this.food, 'count', 1)
          } else {
            this.food.count++
          }
        },
        decreaseCart (event) {
          if (!event._constructed) {
            return
          }
          if (this.food.count) {
            this.food.count--
          }
        }
      }
    }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    position relative
    .cart-decrease
      display inline-block
      line-height 18px
      font-size 22px
      margin 6px
      height 18px
      width 18px
      color white
      vertical-align center
      background dodgerblue
      border-radius 50%
      span
        position absolute
        display inline-block
        height 16px
        width 16px
        top 5px
        left 10px
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 4px
      line-height 24px
      text-align top
      font-size 10px
      color rgb(147, 153, 159)
    .cart-add
      display inline-block
      line-height 18px
      font-size 22px
      margin 6px
      height 18px
      width 18px
      color white
      vertical-align center
      background dodgerblue
      border-radius 50%
      span
        position absolute
        display inline-block
        height 16px
        width 16px
        top 5px
        right 7px
</style>
