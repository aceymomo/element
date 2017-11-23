<template>
  <div class="cartconcontrol">
      <transition name="fade">
          <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart"></div>
      </transition>
      <transition name="fade">
          <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      </transition>
      <div class="cart-add icon-add_circle" @click="addCart($event)"></div>
  </div>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue'
  export default {
    props:{
        food:{
            type:Object
        }
    },
    data() {
      return {

      }

    },
    components: {

    },
    methods:{
        addCart(event){
            if(!event._constructed){
                return
            }
            if(!this.food.count){
                Vue.set(this.food,'count',1)
            }else{
                this.food.count ++
            }
        },
        decreaseCart(){
            if(!event._constructed){
                return
            }
            if(this.food.count){
                this.food.count --
            }
        }
    }
  }
</script>

<style scoped lang="stylus">
.cartconcontrol
    display :flex
    font-size:0
    .fade-enter
        opacity :0
        transform :translate3d(24px,0,0) rotate(180deg)
    .fade-enter-active,.fade-leave-active
        transition :all 0.4s linear
    .fade-leave-to
        transform :translate3d(24px,0,0) rotate(180deg)
        opacity:0
    .cart-decrease,.cart-add
        display :flex
        padding:6px
        font-size:24px
        line-height :24px
        color:rgb(0,160,220)
    .cart-count
        display :flex
        width:12px
        padding-top :6px
        line-height :24px
        text-align :center
        font-size:10px
        color:rgb(147,153,159)
</style>
