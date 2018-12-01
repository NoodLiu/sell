<template>
      <div class="cartcontrol">
        <div class="icon-remove_circle_outline" v-show="food.count" @click="reduce"></div>
        <div class="count" v-show="food.count">{{food.count}}</div>
        <div class="icon-add_circle" @click="add"></div>
      </div>
</template>

<script>
  import Vue from 'vue'
    export default {
        name: "cartcontrol",
        props:{
          food:{
            type:Object
          }
        },
      methods:{
        reduce(){
          this.food.count--
        },
        add(){
          if (!this.food.count) {
            Vue.set(this.food,'count',0)
            this.food.count++
          }else {
            this.food.count++
          }
        }
      }
    }
</script>

<style lang="stylus" scoped>
  .cartcontrol
    div
      vertical-align top
      display inline-block
    .icon-remove_circle_outline
      font-size 24px
      line-height 24px
      color rgb(0 160 220)
    .count
      font-size 10px
      line-height 24px
      color rgb(147 153 159)
      width 24px
      line-height 24px
      text-align center
    .icon-add_circle
      font-size 24px
      line-height 24px
      color rgb(0 160 220)
</style>
