<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link :to="{ path: '/goods' }">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{ path: '/ratings' }">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{ path: '/seller' }">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from './components/header/header'

export default {
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then((response) => {
      response = response.body
      if (response.errno === 0) {
        this.seller = response.data
      }
    })
  },
  components: { 'v-header': header }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import './common/css/mixin.styl'

  .tab{display:flex;width:100%;height:40px;line-height:40px;border-1px(rgba(7,17,27,0.2));}
  .tab-item{flex:1;text-align:center;}
  .tab-item>a{display:block;font-size:14px;color:rgb(77,85,93)}
  .tab-item>a.active{color:#E31B1E;}
  .tab-item.active{color:blue;}
</style>