<template>
<div>
  <transition name="slide">
      <div v-show="foodList" class="food" ref="food">
          <div class="food-content">
              <div class="image-header">
                  <img :src="food.image">
                  <div class="back" @click.stop="hideBox">
                      <i class="icon-arrow_lift"></i>
                  </div>
              </div>
              <div class="content">
                  <h1 class="title">{{food.name}}</h1>
                  <div class="detail">
                      <span class="sell-count">月售{{food.sellCount}}份</span>
                      <span class="rating">好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                      <span class="now"><i>￥</i>{{food.price}}</span><span class="old" v-show="food.oldPrice"><i>￥</i>{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                      <cartcontrol :food="food" v-show="food.count || food.count !== 0"></cartcontrol>
                  </div>
                  <div class="buy" v-show="!food.count || food.count === 0" @click.stop="buyClick($event)">加入购物车</div>
              </div>
              <split v-show="food.info">
                  <div class="info" v-show="food.info">
                      <h1 class="title">商品信息</h1>
                      <p class="text">{{food.info}}</p>
                  </div>
              </split>
          </div>
      </div>
  </transition>
</div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import Vue from 'vue'
import cartcontrol from '@/components/cartconcontrol/cartconcontrol'
import split from '@/components/split/split'
  export default {
    props:{
        food:{
            type:Object
        }
    },
    data() {
      return {
          foodList:false
      }

    },
    methods:{
        show(){
            this.foodList = true
            this.$nextTick(()=>{
                if(!this.scroll){
                    this.scroll = new BScroll(this.$refs.food,{
                        click:true
                    })
                }else{
                    this.scroll.refresh()
                }
            })
        },
        hideBox(){
            this.foodList = false
        },
        buyClick(event){
            if(!event._constructed){
                return
            }
            Vue.set(this.food,'count',1)
        }
    },
    components: {
        cartcontrol,
        split
    }
  }
</script>

<style scoped lang="stylus">
.slide-enter
    opacity :0
    transform :translate3d(100%,0,0)
.slide-enter-active,.slide-leave-active
    transition :all 0.2s
.slide-leave-to
    transform :translate3d(100%,0,0)
    opacity:0
.food
    position:fixed
    top:0
    left:0
    bottom:48px
    width:100%
    background :#f3f5f7
    .food-content
        .image-header
            position :relative
            width:100%
            height:0
            padding-top:100%
            img
                position :absolute
                top:0
                left:0
                width:100%
                height:100%
            .back
                position :absolute
                top:10px
                left:0
                .icon-arrow_lift
                    display :block
                    padding:10px
                    font-size:20px
                    color:#fff
        .content
            background :#fff
            padding:18px
            font-size:0
            position :relative
            .title
                font-size:14px
                font-weight :700
                color:rgb(7,17,27)
                line-height :14px
            .detail
                display :flex
                margin-top:8px
                .sell-count
                    margin-right :12px
                    font-size:10px
                    color:rgb(147,153,159)
                    line-height :10px
                .rating
                    font-size:10px
                    color:rgb(147,153,159)
                    line-height :10px
            .price
                display :flex
                margin-top:18px
                .now
                    font-size :14px
                    font-weight :700
                    color:rgb(240,20,20)
                    line-height :24px
                    margin-right:12px
                    i 
                        font-size :10px
                        font-weight :normal
                        font-style :normal
                .old
                    font-size :10px
                    font-weight :700
                    color:rgb(147,153,159)
                    line-height :24px
                    i 
                        font-weight :normal
                        text-decoration :line-through
                        font-style :normal
            .cartcontrol-wrapper
                position :absolute
                right:12px
                bottom:12px
            .buy
                position :absolute
                right:18px
                bottom:18px
                height:24px
                line-height :24px
                text-align :center
                background :rgb(0,160,220)
                border-radius :12px
                font-size:10px
                color:#fff
                padding:0 12px
        .info
            padding:18px
            .title
                line-height :14px
                margin-bottom:6px
                font-size:14px
                color:rgb(7,17,27)
            .text
                font-size:12px
                font-weight :200
                color:rgb(77,85,93)
                line-height :24px
                padding-left:8px
</style>
