<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link :to="{path:'/goods'}">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{path:'/ratings'}">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link :to="{path:'/seller'}">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
import vHeader from '@/components/header/header'
import {urlParse} from '@/common/js/util'
const ERR_OK = 0
export default {
  name: 'app',
  components:{
    vHeader
  },
  data(){
    return{
      seller:{
        id:(()=>{
          let queryParam = urlParse()
          return queryParam.id 
        })()
      }
    }
  },
  created(){
    this.$http.get('/api/seller?id=' + this.seller.id)
    .then(res=>{
      if(res.data.errno === ERR_OK){
        this.seller = Object.assign({},this.seller , res.data.data)
      }
    })
    .catch(err=>{
      console.log(err)
    })
  },
  mounted(){
    this.$router.push({path:'/goods'})
  }
}
</script>

<style lang="stylus">
@import './common/stylus/mixin'
#app 
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  color: #2c3e50
  .tab
    display:flex
    width:100%
    height :40px
    line-height :40px
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      flex:1
      text-align :center
      & > a
        display:block
        color:rgb(77,85,93)
        &.router-link-active
          color:rgb(240,20,20)
</style>
