<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item border-1px">
        <span class="text">
          <span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}aa
        </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">
const ERR_OK = 0
  export default {
    props:{
      seller:{
        type:Object
      }
    },
    data() {
      return {
        goods:[]
      }

    },
    created(){
      this.$http.get('/api/goods')
      .then(res=>{
        if(res.data.errno === ERR_OK){
          this.goods = res.data.data
        }
      })
      .catch(err=>{
        console.log(err)
      }),
      this.classMap = ['decrease','discount','guarantee','invoice','special']
    },
    components: {

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
</style>
