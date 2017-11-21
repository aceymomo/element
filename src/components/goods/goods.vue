<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item border-1px" :class="{'current':currentIndex === index}" @click="selectMenu(index,$event)">
          <span class="text">
            <span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="nowprice"><i>￥</i>{{food.price}}</span><span class="oldprice" v-show="food.oldPrice"><i>￥</i>{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart></shopcart>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import shopcart from '@/components/shopcart/shopcart'
const ERR_OK = 0
  export default {
    props:{
      seller:{
        type:Object
      }
    },
    data() {
      return {
        goods:[],
        listHeight:[],
        scrollY:0
      }

    },
    created(){
      this.$http.get('/api/goods')
      .then(res=>{
        if(res.data.errno === ERR_OK){
          this.goods = res.data.data
          this.$nextTick(()=>{
            this._initScroll()
            this._calcuateHeight()
          })
        }
      })
      .catch(err=>{
        console.log(err)
      }),
      this.classMap = ['decrease','discount','guarantee','invoice','special']
    },
    computed:{
      currentIndex(){
        for(let i=0;i<this.listHeight.length;i++){
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if(!height2 || (this.scrollY>=height1 && this.scrollY<height2)){
            return i
          }
        }
        return 0
      }
    },
    methods:{
      selectMenu(index,event){
        if(!event._constructed){
          return
        }
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el,300)
      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuWrapper,{
          click:true
        })

        this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
          probeType:3
        })

        this.foodsScroll.on('scroll',(pos)=>{
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calcuateHeight(){
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
        let height = 0
        this.listHeight.push(height)
        for(let i=0;i<foodList.length;i++){
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      }
    },
    components: {
      shopcart
    }
  }
</script>

<style slot-scope=”scope” lang="stylus">
@import '../../common/stylus/mixin'
.goods
  display :flex
  position :absolute
  top:174px
  bottom:46px
  width:100%
  overflow :hidden
  .menu-wrapper
    flex:0 0 80px
    width:80px
    background :#f3f5f7
    .menu-item
      display :flex
      align-items :center
      text-align :center
      padding:0 12px
      height:54px
      width:56px
      line-height :14px
      border-1px(rgba(7,17,27,0.1))
      &.current
        position :relative
        z-index :10
        margin-top:-1px
        background :#fff
        font-weight :700
        border-none()
      .icon
        display :inline-block
        vertical-align :top
        width:12px
        height :12px
        margin-right :2px
        background-size :12px 12px
        background-repeat :no-repeat
        &.decrease
          bg-image('decrease_3')
        &.discount
          bg-image('discount_3')
        &.guarantee
          bg-image('guarantee_3')
        &.invoice
          bg-image('invoice_3')
        &.special
          bg-image('special_3')
      .text
        width:56px
        font-size:12px
  .foods-wrapper
    flex:1
    font-size:0
    .food-list
      width:100%
      .title
        height:26px
        line-height :26px
        background:#f3f5f7
        color:rgb(147,153,159)
        font-size:12px
        border-left:2px solid #d9dde1
        padding-left:14px
      .food-item
        display :flex
        background:#fff
        margin:18px
        padding-bottom:18px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
          margin-bottom:0
        .icon
          flex:0 0 57px
          margin-right:10px
        .content
          padding:0
          .name
            height :14px
            font-size:14px
            color:rgb(7,17,27)
            line-height :14px
            margin-top:2px
          .desc,.extra
            margin-top:8px
            font-size:10px
            color:rgb(147,153,159)
            line-height :12px
          .extra
            font-size:0
            span
              font-size:10px
              margin-right:12px
          .price
            .nowprice
              font-size:14px
              color:rgb(240,20,20)
              font-weight :700
              line-height :24px
              i
                font-style:normal
                font-size:10px
            .oldprice
              margin-left:8px
              font-size:10px
              color:rgb(147,153,159)
              font-weight :700
              line-height :24px
              text-decoration :line-through
              i
                font-style:normal
</style>
