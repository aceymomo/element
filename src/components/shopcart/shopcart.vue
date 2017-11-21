<template>
  <div class="shopcart">
      <div class="content">
          <div class="content-left">
              <div class="logo-wrapper">
                  <div class="logo">
                      <span class="icon-shopping_cart"></span>
                  </div>
              </div>
              <div class="price"></div>
              <div class="desc"></div>
          </div>
          <div class="content-right"></div>
      </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
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
        .content-left
            flex:1

        .content-right
            flex:0 0 105px
            width:105px
</style>
