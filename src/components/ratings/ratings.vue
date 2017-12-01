<template>
 <div class="ratings" ref="ratings">
     <div class="ratings-content">
       <div class="overview">
         <div class="overview-left">
           <h1 class="title">{{seller.score}}</h1>
           <p class="scoring">综合评分</p>
           <span class="statistics">高于周边商家{{seller.rankRate}}%</span>
         </div>
         <div class="overview-right">
           <div class="score-wrapper">
             <span class="title">服务态度</span>
             <star :size="36" :score="seller.serviceScore"></star>
             <span class="score">{{seller.serviceScore}}</span>
           </div>
           <div class="score-wrapper">
             <span class="title">商品评分</span>
             <star :size="36" :score="seller.foodScore"></star>
             <span class="score">{{seller.foodScore}}</span>
           </div>
           <div class="score-box">
             <span class="title">送达时间</span>
             <span class="time">{{seller.deliveryTime}}分钟</span>
           </div>
         </div>
       </div>
       <split></split>
       <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc"
        :ratings="ratings" @ratingtype="ratingtypes" @toggletype="toggletypes"></ratingselect>
       <div class="rating-wrapper">
         <ul>
           <li v-for="rating in ratings" class="rating-item" v-show="needShow(rating.rateType,rating.text)">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <div class="usertime">
                <h1 class="title">{{rating.username}}</h1>
                <span class="time">{{rating.rateTime | formatData}}</span>
              </div>
              <div class="startime">
                <star :size="24" :score="rating.score"></star>
                <span class="times" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="iconbox">
                <i class="icon-thumb_up" v-show="rating.recommend"></i>
                <ul class="recomcon">
                  <li v-show="rating.recommend" v-for="recom in rating.recommend" class="recomBox">
                    <span class="recomment">{{recom}}</span>
                  </li>
                </ul>
              </div>
            </div>
           </li>
         </ul>
       </div>
     </div>
 </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import star from '@/components/star/star'
import split from '@/components/split/split'
import ratingselect from '@/components/ratingselect/ratingselect'
import {formatData} from '@/common/js/date'
const ALL = 2
const ERR_OK = 0
 export default {
   props:{
     seller:{
       type:Object
     }
   },
   data() {
     return {
        ratings:[],
        selectType:ALL,
        onlyContent:true,
        desc:{
            all:'全部',
            positive:'满意',
            negative:'不满意'
        }
     }

   },
   created(){
     this.$http.get('/api/ratings')
     .then(res=>{
       if(res.data.errno === ERR_OK){
         this.ratings = res.data.data
         this.$nextTick(()=>{
           this.scroll = new BScroll(this.$refs.ratings,{
             click:true
           })
         })
       }
     })
     .catch(error=>{
       console.log(error)
     })
   },
   filters:{
        formatData(time){
            let data = new Date(time)
            return formatData(data,'yyyy-MM-dd hh:mm')
        }
    },
   methods:{
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
   components: {
     star,
     split,
     ratingselect
   }
 }
</script>

<style scoped lang="stylus">
@import '../../common/stylus/mixin'
.ratings
  position :absolute
  top:174px
  bottom:0
  left:0
  width:100%
  overflow :hidden
  .overview
    display :flex
    padding:18px 0
    .overview-left
      flex:0 0 137px
      width:137px
      border-right:1px solid rgba(7,17,27,0.1)
      font-size:0
      text-align :center
      padding:6px 0
      @media only screen and (max-width:320px)
        flex:0 0 120px
        width:120px
      .title
        font-size:24px
        color:rgb(255,153,0)
        line-height :28px
        margin-bottom :6px
      .scoring
        font-size:12px
        color:rgb(7,17,27)
        line-height :12px
        margin-bottom :8px
      .statistics
        font-size:10px
        color:rgb(147,153,159)
        line-height :10px
    .overview-right
      flex:1
      padding:6px 24px
      @media only screen and (max-width:320px)
        padding:6px 2px
      .score-wrapper
        display :flex
        font-size:0
        margin-bottom:8px
        .title
          font-size:12px
          color:rgb(7,17,27)
          line-height :18px
          margin-right :12px
        .score
          font-size:12px
          color:rgb(255,153,0)
          line-height :18px
          margin-left :12px
      .score-box
        font-size:0
        .title
          font-size:12px
          color:rgb(7,17,27)
          line-height :18px
          margin-right :12px
        .time
          font-size:12px
          color:rgb(147,153,159)
          line-height :18px
  .rating-wrapper
    padding:0 18px
    .rating-item
      display :flex
      padding:18px 0
      border-1px(rgba(7,17,27,0.1))
      .avatar
        flex:0 0 28px
        width:28px
        margin-right:12px
        img
          border-radius :50%
      .content
        flex:1
        font-size:0
        .usertime
          display :flex
          margin-bottom:4px
          .title
            flex:1
            font-size:10px
            color:rgb(7,17,27)
            line-height :12px
          .time
            font-size:10px
            font-weight :200
            color:rgb(147,153,159)
            line-height :12px
        .startime
          display :flex
          margin-bottom:6px
          .times
            margin-left :6px
            font-size:10px
            font-weight :200
            color:rgb(147,153,159)
            line-height :12px
        .text
          font-size:12px
          color:rgb(7,17,27)
          line-height :18px
          margin-bottom :8px
        .iconbox
          display :flex
          .icon-thumb_up
            font-size:12px
            color:rgb(0,160,220)
            line-height :22px
            margin-right :8px
          .recomcon
            display:flex
            flex-wrap:wrap
          .recomBox
            margin-right:8px
            margin-bottom :10px
            font-size:0
            .recomment
              padding:4px 6px
              font-size:9px
              color:rgb(147,153,159)
              line-height :16px
              border:1px solid rgba(7,17,27,0.1)
              border-radius :2px
</style>
