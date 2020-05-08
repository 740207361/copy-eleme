<template>
  <div id="app">
    <vheader :seller="seller"></vheader>
    <div class="tab">
      <router-link class="tab-item" to="/goods" :class="{'active': currentSort===1}" @click.native="active($event)">商品</router-link>
      <router-link class="tab-item" to="/ratings" :class="{'active': currentSort===2}" @click.native="active($event)">评论</router-link>
      <router-link class="tab-item" to="/seller" :class="{'active': currentSort===3}" @click.native="active($event)">商家</router-link>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import vheader from './components/header/header'
  const ERR_OK = 0
  export default {
    data () {
      return {
        seller: {},
        currentSort: 0
      }
    },
    components: {
      vheader
    },
    created () {
      this.$http.get('/api/seller').then((res) => {
        res = res.body
        if (res.errno === ERR_OK) {
          this.seller = res.data
        }
      })
    },
    methods: {
      active (event) {
        let el = event.currentTarget
        let text = el.innerText
        if (text === '商品') {
          this.currentSort = 1
        } else if (text === '评论') {
          this.currentSort = 2
        } else if (text === '商家') {
          this.currentSort = 3
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      border-bottom 1px solid rgba(7, 17, 27, 0.1)
      .tab-item
        flex: 1
        text-align center
        &.active
          color: red
</style>
