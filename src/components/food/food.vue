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
              <split v-show="food.info"></split>
              <div class="info" v-show="food.info">
                  <h1 class="title">商品信息</h1>
                  <p class="text">{{food.info}}</p>
              </div>
              <split></split>
              <div class="ratingBox">
                  <h1 class="title">商品评价</h1>
                  <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings" @ratingtype="ratingtypes" @toggletype="toggletypes"></ratingselect>
                  <div class="rating-wrapper">
                      <ul v-show="food.ratings && food.ratings.length">
                          <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
                              <div class="user">
                                  <div class="time">{{rating.rateTime | formatData}}</div>
                                  <span class="name">{{rating.username}}</span>
                                  <img class="avatar" width="12" height="12" :src="rating.avatar">
                              </div>
                              <p class="text">
                                  <span :class="{'icon-thumb_up':rating.rateType === 0,'icon-thumb_down':rating.rateType === 1}"></span>{{rating.text}}
                              </p>
                          </li>
                      </ul>
                      <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
                  </div>
              </div>
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
import ratingselect from '@/components/ratingselect/ratingselect'
import {formatData} from '@/common/js/date'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

  export default {
    props:{
        food:{
            type:Object
        }
    },
    data() {
      return {
          foodList:false,
          selectType:ALL,
          onlyContent:true,
          desc:{
              all:'全部',
              positive:'推荐',
              negative:'吐槽'
          }
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
        },
        ratingtypes(val){
            this.selectType = val
            this.$nextTick(()=>{
                this.scroll.refresh()
            })
        },
        toggletypes(){
            this.onlyContent = !this.onlyContent
            this.$nextTick(()=>{
                this.scroll.refresh()
            })
        },
        needShow(type,text){
            if(this.onlyContent && !text){
                return false
            }
            if(this.selectType === ALL){
                return true
            }else{
                return type === this.selectType
            }
        }
    },
    filters:{
        formatData(time){
            let data = new Date(time)
            return formatData(data,'yyyy-MM-dd hh:mm')
        }
    },
    components: {
        cartcontrol,
        split,
        ratingselect
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
        background :#fff
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
            background :#fff
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
        .ratingBox
            background :#fff
            padding:18px 0
            .title
                line-height :14px
                font-size:14px
                color:rgb(7,17,27)
                margin-left:18px
            .rating-wrapper
                .rating-item
                    padding:16px 0
                    margin: 0 18px
                    border-bottom:1px solid rgba(7,17,27,0.1)
                    font-size:0
                    .user
                        display :flex
                        margin-bottom:6px
                        .time
                            flex:1
                            font-size:10px
                            color:rgb(147,153,159)
                            line-height :12px
                        .name
                            margin-right:6px
                            font-size:10px
                            color:rgb(147,153,159)
                            line-height :12px
                        .avatar
                            width:12px
                            height:12px
                            border-radius :50%
                    .text
                        font-size:12px
                        color:rgb(7,17,27)
                        line-height :16px
                        .icon-thumb_up
                            font-size:12px
                            color:rgb(0,160,220)
                            line-height :16px
                            margin-right :4px
                        .icon-thumb_down
                            font-size:12px
                            color:rgb(147,153,159)
                            line-height :16px
                            margin-right :4px
                .no-rating
                    padding:16px 0
                    margin:0 18px
                    font-size:12px
                    color:rgb(147,153,159)
</style>
