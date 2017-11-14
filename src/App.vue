<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
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
    <router-view></router-view>
  </div>
</template>

<script>
import vHeader from '@/components/header/header'
export default {
  name: 'app',
  components:{
    vHeader
  },
  mounted(){
    this.$router.push({path:'/goods'})
  }
}
</script>

<style lang="stylus">
#app 
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px
  .tab
    display:flex
    width:100%;
    height :40px;
    line-height :40px;
    .tab-item
      flex:1
      text-align :center
      & > a
        display:block
        color:rgb(77,85,93)
        &.router-link-active
          color:rgb(240,20,20)
</style>
