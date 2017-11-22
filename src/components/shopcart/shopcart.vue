<template>
  <div class="shopcart">
      <div class="content">
          <div class="content-left">
              <div class="logo-wrapper">
                  <div class="logo">
                      <span class="icon-shopping_cart"></span>
                  </div>
              </div>
              <div class="price">￥0</div>
              <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
          </div>
          <div class="content-right"></div>
      </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props:{
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
    
      }

    },
    components: {

    }
  }
</script>

<style scoped lang="stylus">
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
                    .icon-shopping_cart
                        line-height :44px
                        font-size:24px
                        color:#80858a
            .price
                height:24px
                line-height :24px
                margin:12px 0 0 18px
                padding-right:12px
                font-size:16px
                font-weight :700
                color:rgba(255,255,255,0.4)
                border-right:1px solid rgba(255,255,255,0.1)
            .desc
                line-height :24px
                margin:12px 0 0 12px
                font-size:10px
                font-weight :700
                color:rgba(255,255,255,0.4)
        .content-right
            flex:0 0 105px
            width:105px
</style>
