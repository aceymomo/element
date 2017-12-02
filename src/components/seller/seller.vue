<template>
 <div class="seller" ref="seller">
   <div class="seller-content">
     <div class="overview">
       <h1 class="title">{{seller.name}}</h1>
       <div class="desc">
         <star :size="36" :score="seller.score"></star>
         <span class="text">({{seller.ratingCount}})</span>
         <span class="text">月售{{seller.sellCount}}单</span>
       </div>
       <div class="favorite">
         <span class="icon-favorite" :class="{'active':favorite}" @click="toggleFavorite"></span>
         <span class="text">{{favoriteText}}</span>
       </div>
     </div>
     <div class="overul">
       <ul class="blockbox">
         <li class="block">
           <h2>起送价</h2>
           <div class="content">
             <span class="stress">{{seller.minPrice}}</span>元
           </div>
         </li>
         <li class="block">
           <h2>商家配送</h2>
           <div class="content">
             <span class="stress">{{seller.deliveryPrice}}</span>元
           </div>
         </li>
         <li class="block">
           <h2>平均配送时间</h2>
           <div class="content">
             <span class="stress">{{seller.deliveryTime}}</span>分钟
           </div>
         </li>
       </ul>
     </div>
     <split></split>
     <div class="activity">
       <h1 class="title">公告与活动</h1>
       <p class="text">{{seller.bulletin}}</p>
     </div>
     <ul class="discountbox">
       <li v-for="support in seller.supports" class="discount-wrapper">
         <i class="icon" :class="classMap[support.type]"></i>
         <span class="text">{{support.description}}</span>
       </li>
     </ul>
     <split></split>
     <div class="real">
       <h1 class="title">商家实景</h1>
       <div class="pic-wrapper" ref="realWrapper">
        <ul class="realbox" ref="realList">
          <li v-for="item in seller.pics" class="real-wrapper">
            <img width="120" height="90" :src="item" class="realimg">
          </li>
        </ul>
       </div>
     </div>
     <split></split>
     <div class="describe">
       <h1 class="title">商家信息</h1>
       <ul>
         <li v-for="item in seller.infos" class="desc-item">
           <div class="content">{{item}}</div>
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
import {saveTolocal,loadFromLocal} from '@/common/js/store'
 export default {
   props:{
     seller:{
       type:Object
     }
   },
   data() {
     return {
       favorite: (() => {
          return loadFromLocal(this.seller.id, 'favorite', false);
        })()
     }

   },
   computed:{
     favoriteText(){
      return this.favorite ? '已收藏' : '收藏'
     }
   },
   methods:{
     toggleFavorite(){
       this.favorite = !this.favorite
       saveTolocal(this.seller.id,'favorite',this.favorite)
     }
   },
   created(){
    this.classMap = ['decrease','discount','guarantee','invoice','special']
   },
   mounted(){
    this.$nextTick(()=>{
      if(!this.scroll){
          this.scroll = new BScroll(this.$refs.seller,{
              click:true
          })
      }else{
          this.scroll.refresh()
      }
    })

    if(this.seller.pics){
      let picWidth = 120
      let margin = 6
      let width = (picWidth + margin) * this.seller.pics.length
      this.$refs.realList.style.width = width + 'px'
      this.$nextTick(()=>{
        if(!this.picScroll){
          this.picScroll = new BScroll(this.$refs.realWrapper,{
            scrollX:true,
            eventPassthrough:'vertical'
          })
        }else{
          this.picScroll.refresh()
        }
      })
    }
   },
   components: {
     star,
     split
   }
 }
</script>

<style scoped lang="stylus">
@import '../../common/stylus/mixin'
.seller
  position :fixed
  top:174px
  left:0
  bottom:0
  width:100%
  overflow :hidden
  .seller-content
    margin:18px 0
    .overview
      position :relative
      margin:0 18px
      padding-bottom:18px
      border-1px(rgba(7,17,27,0.1))
      font-size:0
      .title
        font-size:14px
        color:rgb(7,17,27)
        line-height :14px
      .desc
        display :flex
        margin-top :8px
        .star
          line-height :18px
          margin-right :8px
        .text
          font-size:10px
          color:rgb(77,85,93)
          line-height :18px
          margin-right :12px
      .favorite
        position :absolute
        right:18px
        top:0px
        text-align :center
        .icon-favorite
          font-size:24px
          line-height :24px
          color:#d4d6d9
          &.active
            color:rgb(240,20,20)
        .text
          display :block
          margin-top :4px
          font-size:10px
          color:rgb(77,85,93)
          line-height :10px
    .overul
      padding:18px 0
      .blockbox
        display :flex
        justify-content :center
        .block
          flex:1
          border-right:1px solid rgba(7,17,27,0.1)
          text-align :center
          &:last-child
            border:none
          h2
            font-size:10px
            color:rgb(147,153,159)
            line-height :10px
          .content
            margin-top:4px
            font-size:10px
            font-weight :200
            color:rgb(7,17,27)
            line-height :24px
            .stress
              font-size:24px
              font-weight :200
              color:rgb(7,17,27)
              line-height :24px
    .activity
      margin:18px 18px 0 18px
      padding-bottom:16px
      border-1px(rgba(7,17,27,0.1))
      font-size:0
      .title
        margin-bottom :8px
        font-size:14px
        color:rgb(7,17,27)
        line-height :14px
      .text
        margin:0 12px
        font-size:12px
        font-weight :200
        color:rgb(240,20,20)
        line-height :24px
    .discountbox
      padding:0 18px
      .discount-wrapper
        padding:16px 12px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border:none
        .icon
          display :inline-block
          vertical-align :top
          width:16px
          height:16px
          background-size :16px 16px
          background-repeat :no-repeat
          &.decrease
            bg-image('decrease_4')
          &.discount
            bg-image('discount_4')
          &.guarantee
            bg-image('guarantee_4')
          &.invoice
            bg-image('invoice_4')
          &.special
            bg-image('special_4')
        .text
          font-size:12px
          font-weight :200
          color:rgb(7,17,27)
          line-height :16px
    .real
      padding:18px
      .title
        margin-bottom :12px
        font-size:14px
        color:rgb(7,17,27)
        line-height :14px
      .pic-wrapper
        width:100%
        overflow :hidden
        white-space :nowrap
        .realbox
          display :flex
          .real-wrapper
            margin-right:6px
            .realimg
              width:120px
              height:90px
    .describe
      padding:18px
      .title
        padding-bottom :12px
        font-size:14px
        color:rgb(7,17,27)
        line-height :14px
        border-1px(rgba(7,17,27,0.1))
      .desc-item
        padding:16px 12px
        border-1px(rgba(7,17,27,0.1))
        .content
          font-size:12px
          color:rgb(7,17,27)
          font-weight :200
          line-height :16px
</style>
