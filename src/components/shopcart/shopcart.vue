<template>
<div>
  <div class="shopcart">
      <div class="content" @click="toggleList">
          <div class="content-left">
              <div class="logo-wrapper">
                  <div class="logo" :class="{'highlight':totalCount>0}">
                      <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
                  </div>
                  <div class="num" v-show="totalCount>0">{{totalCount}}</div>
              </div>
              <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}元</div>
              <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
          </div>
          <div class="content-right">
              <div class="pay" :class="payClass" @click.stop="pay">{{payDesc}}</div>
          </div>
      </div>
      <transition name="slide">
        <div class="shopcart-list" v-show="listShow">
            <div class="list-header">
                <h1 class="title">购物车</h1>
                <span class="empty" @click="empty">清空</span>
            </div>
            <div class="list-content" ref="listContent">
                <ul>
                    <li class="list-box" v-for="food in selectFood">
                        <span class="name">{{food.name}}</span>
                        <div class="price">
                            <span><i>￥</i>{{food.count*food.price}}</span>
                        </div>
                        <div class="cartcontrol">
                            <cartconcontrol :food="food"></cartconcontrol>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
      </transition>
  </div>
  <div class="list-mask" v-show="listShow" @click="hideList"></div>
</div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import cartconcontrol from '@/components/cartconcontrol/cartconcontrol'
  export default {
    props:{
        selectFood:{
            type:Array,
            default(){
                return [
                    {
                        price:0,
                        count:0
                    }
                ]
            }
        },
        deliveryPrice:{
            type:Number,
            default:0
        },
        minPrice:{
            type:Number,
            default:0
        }
    },
    data() {
      return {
          theShow:true
      }

    },
    computed:{
        totalPrice(){
            let total = 0
            this.selectFood.forEach((food)=>{
                total += food.price * food.count
            })
            return total
        },
        totalCount(){
            let count = 0
            this.selectFood.forEach((food)=>{
                count += food.count
            })
            return count
        },
        payDesc(){
            if(this.totalPrice === 0){
                return `￥${this.minPrice}元起送`
            }else if(this.totalPrice < 20){
                let deff = this.minPrice - this.totalPrice
                return `还差${deff}元起送`
            }else{
                return '去结算'
            }
        },
        payClass(){
            if(this.totalPrice < this.minPrice){
                return 'not-enough'
            }else{
                return 'enough'
            }
        },
        listShow(){
            if(!this.totalCount){
                this.theShow = true
                return false
            }
            let show = !this.theShow
            if(show){
                this.$nextTick(()=>{
                    if(!this.scroll){
                        this.scroll = new BScroll(this.$refs.listContent,{
                            click:true
                        })
                    }else{
                        this.scroll.refresh()
                    }
                })
            }
            return show
        }
    },
    methods:{
        empty(){
            this.selectFood.forEach((food)=>{
                food.count = 0
            })
        },
        toggleList(){
            if(!this.totalCount){
                return
            }
            this.theShow = !this.theShow
        },
        hideList(){
            this.theShow = true
        },
        pay(){
            if(this.totalPrice<this.minPrice){
                return
            }
            window.alert(`需要支付${this.totalPrice}元`)
        }
    },
    components: {
        cartconcontrol
    }
  }
</script>

<style scoped lang="stylus">
@import '../../common/stylus/mixin'
.shopcart
    position :fixed
    left:0
    bottom:0
    width:100%
    height:48px
    z-index :50
    .content
        display :flex 
        background :#141d27
        font-size:0
        .content-left
            flex:1
            display :flex
            height:58px
            z-index:100
            .logo-wrapper
                display:flex
                align-items :center
                justify-content :center
                position :relative
                top:-10px
                width:56px
                height 56px
                background :#141d27
                border-radius :50%
                margin-left:18px
                .logo
                    width:44px
                    height:44px
                    text-align :center
                    background :#2b343c
                    border-radius :50%
                    &.highlight
                        background :rgb(0,160,220)
                    .icon-shopping_cart
                        line-height :44px
                        font-size:24px
                        color:#80858a
                        &.highlight
                            color:#fff
                .num
                    position :absolute
                    top:0
                    right:0
                    width:24px
                    height:16px
                    line-height :16px
                    text-align :center
                    border-radius :16px
                    font-size:9px
                    font-weight :700
                    color:#fff
                    background :rgb(240,20,20)
                    box-shadow :0 4px 8px 0 rgba(0,0,0,0.4)
            .price
                height:24px
                line-height :24px
                margin:12px 0 0 18px
                padding-right:12px
                font-size:16px
                font-weight :700
                color:rgba(255,255,255,0.4)
                border-right:1px solid rgba(255,255,255,0.1)
                &.highlight
                    color:#fff
            .desc
                line-height :24px
                margin:12px 0 0 12px
                font-size:10px
                font-weight :700
                color:rgba(255,255,255,0.4)
        .content-right
            flex:0 0 105px
            width:105px
            .pay
                height:48px
                line-height :48px
                text-align :center
                font-size:12px
                color:rgba(255,255,255,0.4)
                font-weight :700
                &.not-enough
                    background :#2b343c
                &.enough
                    background :#00b43c
                    color:#fff
    .slide-enter
        opacity :0
        transform :translate3d(0,100%,0)
    .slide-enter-active,.slide-leave-active
        transition :all 0.2s
    .slide-leave-to
        transform :translate3d(0,100%,0)
        opacity:0
    .shopcart-list
        width:100%
        position :absolute
        left:0
        bottom:48px
        .list-header
            display :flex
            justify-content :space-between
            height:40px
            padding:0 18px
            font-size:0
            background :#f3f5f7
            border-1px(rgba(7,17,27,0.1))
            .title
                font-size:14px
                font-weight :200
                color:rgb(7,17,27)
                line-height :40px
            .empty
                font-size:12px
                color:rgb(0,160,220)
                line-height :40px
        .list-content
            max-height:200px
            overflow :hidden
            padding:0 18px
            background :#fff
            .list-box
                height:48px
                display :flex
                align-items :center
                border-1px(rgba(7,17,27,0.1))
                .name
                    display :inline-block
                    font-size:14px
                    color:rgb(7,17,27)
                    line-height :24px
                    flex:1
                .price
                    line-height :24px
                    margin-right :12px
                    span
                        font-size:14px
                        font-weight :700
                        color:rgb(240,20,20)
                        i
                            font-size:10px
                            font-weight :normal
                            font-style :normal
                .cartcontrol
                    float:right
.list-mask
    position :fixed
    top:0
    left:0
    width:100%
    height:100%
    z-index :40
    background :rgba(7,17,27,0.6)
    backdrop-filter: blur(10px)
</style>
