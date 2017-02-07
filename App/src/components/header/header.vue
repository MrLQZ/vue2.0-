<template>
  <div class="header">
  	<div class="content-wrapper">
  		<div class="avatar">
			<img width="65" height="65" :src="seller.avatar">
  		</div>
  		<div class="content">
			<div class="title">
				<span class="brand"></span>
				<span class="name">{{seller.name}}</span>
  			</div>
  			<div class="description">
  				{{seller.description}}/{{seller.deliveryTime}}分钟送达
  			</div>
  			<div v-if="seller.supports" class="support">
  				<span class="icon" :class="classMap[seller.supports[0].type]"></span>
  				<span class="text">{{seller.supports[0].description}}</span>
  			</div>
  			<div v-if="seller.supports" class="support-count" @click="showDetail">
  				<span class="count">{{seller.supports.length}}个</span>
  				<i class="icon-keyboard_arrow_right"></i>
  			</div>
  		</div>
  	</div>
  	<div class="bulletin-wrapper" @click="showDetail">
  		<span class="bulletin-title"></span>
  		<span class="bulletin-text">{{ seller.bulletin}}</span>
  		<i class="icon-keyboard_arrow_right"></i>
  	</div> 
  	<div class="background">
  		<img :src="seller.avatar" width="100%" height="100%">
  	</div>
  	<div v-show="detailShow" class="detail" >
      <transition-group name="fade" tag="div">
    		<div class="detail-wrapper clearfix" key="div">
    			<div class="detail-main">
    				<h1 class="name">{{ seller.name}}</h1>
    				<div class="star-wrapper">
    					<star :size="48" :score="seller.score"></star>
    				</div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[seller.supports[item.type].type]"></span>
                <span class="text">{{seller.supports[item.type].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家供告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin }}</p>
            </div>
    			</div>
    		</div>
    		<div class="detail-close" @click="hiddenDetail" key="div">
    			<i class="icon-close"></i>
    		</div>
      </transition-group>
  	</div>
  </div>
</template>

<script type="text/javascript">
import star from 'components/star/star'
export default {
  props: {seller: { type: Object }},
  created () { this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'] },
  data () { return {detailShow: false} },
  methods: {showDetail () { this.detailShow = true }, hiddenDetail () { this.detailShow = false }},
  components: { star }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  	@import '../../common/css/mixin'

	.header{color:#fff;position:relative;overflow:hidden;background:rgba(7,17,27,0.5)}
	.content-wrapper{position:relative;padding:24px 12px 18px 24px;font-size:0;}
	.avatar{display:inline-block;vertical-align:top}
	.avatar img{border-radius:2px;}
	.content{display:inline-block;margin-left:16px;}
	.title{margin:2px 0 8px 0;}
	.brand{width:30px;height:18px;display:inline-block;vertical-align:top;bg-image('brand');background-size:30px 18px;background-repeat:no-repeat;}
	.name{margin-left:6px;font-size:16px;line-height:18px;font-weight:bold;}
	.description{margin-bottom:10px;line-height:12px;font-size:12px}
	.support .icon{display:inline-block;vertical-align:top;width:12px;height:12px;margin-right:4px;background-size:12px 12px;background-repeat:no-repeat;}
	.decrease{bg-image('decrease_1');width:12px;}
	.discount{bg-image('discount_1');width:12px;}
	.special{bg-image('special_1');width:12px;}
	.invoice{bg-image('invoice_1');width:12px;}
	.guarantee{bg-image('guarantee_1');width:12px;}
	.text{font-size:10px;line-height:12px}
	.support-count{position:absolute;right:12px;bottom:18px;padding:0 8px;height:24px;line-height:24px;border-radius:12px;background:rgba(0,0,0,0.2);}
	.count{font-size:10px;}
	.icon-keyboard_arrow_right{margin-left:2px;line-height:24px;font-size:8px;}

	.bulletin-wrapper{position:relative;height:28px;line-height:28px;padding:0 22px 0 12px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;background:rgba(7,17,27,0.2)}
	.bulletin-title{display:inline-block;width:22px;height:12px;bg-image('bulletin');background-size:22px 12px;background-repeat:no-repeat;vertical-align:top;margin-top:8px}
	.bulletin-text{font-size:10px;margin:0 4px;vertical-align:top;}
	.bulletin-wrapper .icon-keyboard_arrow_right{position:absolute;font-size:10px;top:8px;right:12px;}
	.background{position:absolute;top:0px;left:0px;width:100%;height:100%;z-index:-1; -webkit-filter:blur(10px)}
	.detail{position:fixed;z-index:100;top:0;left:0;width:100%;height:100%;overflow:auto;background:rgba(7,17,27,0.8);background-filter:blur(10px)}
	.detail-wrapper{min-height:100%;width:100%}
	.detail-main{margin-top:40px;padding-bottom:64px;}
	.name{linr0height:16px;text-align:center;font-size:18px;font-weight:700}
	.detail-close{position:relative;width:32px;height:32px;margin:-64px  auto 0 auto;clear:both;font-size:32px;}
	.star-wrapper{margin-top:18px;padding:2px 0;text-align:center}
  .detail-main .title{display:flex;width:80%;margin:25px auto 24px auto;}
  .detail-main .title .line{flex:1;position:relative;top:-6px;border-bottom:2px solid rgba(255,255,255,0.2)}
  .detail-main .title .text{padding:0 12px;font-size:14px;font-weight:700px; }
  .supports{width:80%;margin:0 auto;}
  .support-item{padding:0 12px;margin-bottom:12px;font-size:0}
  .support-item:last-child{margin-bottom:0}
  .icon{display:inline-block;width:16px;height:16px;vertical-align:top;margin-right:6px;background-size:16px 16px;background-repeat:no-repeat; }
 .support-item .text{line-height:16px;font-size:12pxl}
 .bulletin{width:80%;margin:0 auto;}
 .bulletin .content{line-height:24px;font-size:12px;}
</style>