<template>
  <div class="ratingselect">
      <div class="rating-type">
          <span @click="select(2,$event)" class="block positive" :class="{'active':selectType === 2}">{{desc.all}}<i>{{ratings.length}}</i></span>
          <span  @click="select(0,$event)" class="block positive" :class="{'active':selectType === 0}">{{desc.positive}}<i>{{positives.length}}</i></span>
          <span  @click="select(1,$event)" class="block negative" :class="{'active':selectType === 1}">{{desc.negative}}<i>{{negatives.length}}</i></span>
      </div>
      <div @click="toggleContent($event)" class="switch" :class="{'on':onlyContent}">
          <span class="icon-check_circle"></span>
          <span class="text">只看有内容的评价</span>
      </div>
  </div>
</template>
<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
export default{
    data(){
        return{
            selectTypes:this.selectType,
            onlyContents:this.onlyContent
        }
    },
    watch:{
        selectType(val){
            this.selectTypes = val
        },
        selectTypes(val){
            this.$emit('ratingtype',val)
        },
        onlyContent(val){
            this.onlyContents = val
        },
        onlyContents(){
            this.$emit('toggletype',this.onlyContent)
        }
    },
    props:{
        ratings:{
            type:Array,
            default(){
                return []
            }
        },
        selectType:{
            type:Number,
            default:ALL
        },
        onlyContent:{
            type:Boolean,
            default:false
        },
        desc:{
            type:Object,
            default(){
                return{
                    all:'全部',
                    positive:'满意',
                    negative:'不满意'
                }
            }
        }
    },
    computed:{
        positives(){
            return this.ratings.filter((rating)=>{
                return rating.rateType === POSITIVE
            })
        },
        negatives(){
            return this.ratings.filter((rating)=>{
                return rating.rateType === NEGATIVE
            })
        }
    },
    methods:{
        select(val,event){
            if(!event._constructed){
                return
            }
            this.selectTypes = val
        },
        toggleContent(event){
            if(!event._constructed){
                return
            }
            this.onlyContents = !this.onlyContents
        }
    }
}
</script>
<style scoped lang="stylus">
.ratingselect
    .rating-type
        border-bottom:1px solid rgba(7,17,27,0.1)
        padding:18px 0 18px 0
        display :flex
        margin:0 18px
        .block
            display :block
            padding:8px 12px
            font-size:12px
            line-height :16px
            border-radius :2px
            margin-right :8px
            color:rgb(77,85,93)
            &.active
                color:#fff
            i
                font-size:8px
                font-style :normal
                margin-left :6px
            &.positive
                background :rgba(0,160,220,0.2)
                &.active
                    background :rgb(0,160,220)
            &.negative
                background :rgba(77,85,93,0.2)
                &.active
                    background :rgb(77,85,93)
    .switch
        padding:12px 0
        width:100%
        font-size:0
        display :flex
        align-items :center
        border-bottom:2px solid rgba(7,17,27,0.1)
        &.on
            .icon-check_circle
                color:#00c850
        .icon-check_circle
            margin-left:18px
            font-size :24px
            color:rgb(147,153,159)
            line-height :24px
        .text
            margin-left :4px
            font-size:12px
            color:rgb(147,153,159)
            line-height :24px
</style>
