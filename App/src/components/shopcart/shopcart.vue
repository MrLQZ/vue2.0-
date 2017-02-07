<template>
	<div>
	  <div class="shopcart">
	    <div class="content" @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight':totalCount>0}">
						<i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}元</div>
			</div>
			<div class="content-right" @click.stop.prevet="pay">
				<div class="pay" :class="payClass">{{payDesc}}</div>
			</div>
	    </div>
	    <div class="shopcart-list" v-show="listShow">
			<div class="list-header">
				<h1 class="title">购物车</h1>
				<span class="empty" @click="empty">清空</span>
			</div>
			<div class="list-content" ref="listContent">
				<ul>
					<li class="food" v-for="food in selectFoods">
						<span class="name">{{food.name}}</span>
						<div class="price">
							<span>￥{{food.price*food.count}}</span>
						</div>
						<div class="cartcontrol-wrapper">
							<cartcontrol :food="food"></cartcontrol>
						</div>
					</li>
				</ul>
			</div>
	    </div>
	  </div>
	  <div class="list-mask" v-show="listShow" @click="hideList"></div>
	</div>
</template>

<script type="text/javascript">
import cartcontrol from 'components/cartcontrol/cartcontrol'
import BScroll from 'better-scroll'
export default {
  props: {
    selectFoods: {
      type: Array,
      default () {
        return [
          {
            price: 10,
            count: 4
          }
        ]
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      fold: true
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}元起送`
      } else {
        return '去结算'
      }
    },
    payClass () {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    },
    listShow () {
      if (!this.totalCount) {
        this.fold = true
        return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listContent, { click: true })
          } else {
            this.scroll.refresh()
          }
        })
      }
      return show
    }
  },
  components: {
    cartcontrol
  },
  methods: {
    toggleList () {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    hideList () {
      this.fold = true
    },
    empty () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    },
    pay () {
      if (this.totalPrice < this.minPrice) {
        return
      }
      window.alert(`支付${this.totalPrice + this.deliveryPrice}元`)
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/css/mixin.styl'

.shopcart
	position:fixed
	left:0
	bottom:0
	width:100%
	height:48px
	z-index:50
	background:#141d27
	.desc, .price,.pay
		color:rgba(255,255,255,0.4)
	.content
		display:flex
		background:#141d27
		margin-left:0
		font-size:0
		.content-left
			flex:1
			.logo-wrapper
				display:inline-block
				vertical-align:top
				position:relative
				top:-10px
				padding:6px
				margin-left:10px
				width:56px
				height:56px
				box-sizing:border-box
				border-radius:50%
				background:#141d27
				.logo
					width:100%
					height:100%
					border-radius:50%
					background:#2b343c
					text-align:center
					&.highlight
						background:rgb(0,160,220)
					.icon-shopping_cart
						line-height:44px
						font-size:28px
						color:#80858a
						&.highlight
							color:#fff
				.num
					position:absolute
					top:0
					right:0
					width:24px
					height:16px
					line-height:16px
					border-radius:16px
					font-size:9px
					font-weight:700
					color:#fff
					background:rgb(240,20,20)
					box-shadow:0 4px 8px 0 rgba(0,0,0,0.4)
					text-align:center
			.price
				display:inline-block
				vertical-align:top
				margin-top:12px
				line-height:24px
				padding-right:6px
				box-sizing:border-box
				border-right:1px solid rgba(255,255,255,0.1)
				font-size:16px
				font-weight:700
				&.highlight
					color:#fff
			.desc
				display:inline-block
				vertical-align:top
				margin:12px 0 0 6px
				line-height:24px
				font-size:12px
		.content-right
			flex:0 0 105px
			width:105px
			background:#2b333b
			.pay
				height:48px
				line-height:48px
				text-align:center
				font-size:12px
				font-weight:700px
				&.not-enough
					background:#2b333b
				&.enough
					background:#00b43c
					color:#fff
	.shopcart-list
		position:absolute
		left:0
		bottom:48px
		z-index:-1
		width:100%
		.list-header
			height:40px
			line-height:40px
			padding:0 18px
			background:#f3f5f7
			border-top:1px solid rgba(7,1,27,0.1)
			border-bottom:1px solid rgba(7,1,27,0.1)
			.title
				margin:0
				float:left
				font-size:14px
				color:rgb(7,17,27)
			.empty
				float:right
				font-size:12px
				color:rgb(0,160,220)
		.list-content
			width:100%
			padding:0 18px
			max-height:217px
			overflow:hidden
			background:#fff
			.food
				position:relative
				padding:12px 0
				box-sizing:border-box
				border-1px(rgba(7,1,27,0.1))
				.name
					line-height:24px
					font-size:14px
					color: rgb(7,17,27)
				.price
					position:absolute
					right:150px
					bottom:12px
					line-height:24px
					font-size:14px
					font-weight:700
					color:rgb(240,20,20)
				.cartcontrol-wrapper
					position:absolute
					right:40px
					bottom:6px
.list-mask
	position:fixed
	top:0
	left:0
	width:100%
	height:100%
	z-index:40
	background:rgba(7,17,27,0.6)
	backdrop-filter:blur(10px)
</style>