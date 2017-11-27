<template>
  <div class="split">
      <slot></slot>
  </div>
</template>
<script>
export default{}
</script>
<style scoped lang="stylus">
.split
    width:100%
    margin-top:16px
    border-top:1px solid rgba(7,17,27,0.1)
    border-bottom:1px solid rgba(7,17,27,0.1)
    background :#fff
</style>
