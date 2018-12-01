<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left" @click="togglePopShow">
        <div class="logo-wrapper"> //黑色背景
          <div class="logo" :class="blue">    //背景图
            <span class="icon-shopping_cart" :class="{'cart':totalPrice>0}"></span>
            <span v-show="totalCount" class="count">{{totalCount}}</span>
          </div>
        </div>
        <div class="price" :class="light">￥{{totalPrice}}</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="payClass">
        {{payContent}}
      </div>
    </div>
    <div class="aaa" v-show="popShow">
    <div class="pop-wrapper" v-show="popShow">
      <div class="foods-wrapper" ref="foodWrepper">
        <div class="title">
          <span class="cart">购物车</span>
          <span class="clear" @click="clearFoodList">清空</span>
        </div>
        <div class="food-content" ref="foodContent">
          <ul class="foods">
            <li v-for="food in selectFoods" class="food border-1px">
              <div class="name">{{food.name}}</div>
              <div class="right">
                <div class="price">￥{{food.price}}</div>
                <div class="cartcontrol-wrapper"><cartcontrol :food="food"></cartcontrol></div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
  import cartcontrol from '../cartcontrol/cartcontrol'
  import BScroller from 'better-scroll'
  export default {
    name: "shopcart",
    data(){
      return{
        popShow:false
      }

    },
    components:{
      cartcontrol
    },
    created(){
        this.$nextTick(()=>{
          this.scroll = new BScroller(this.$refs.foodContent,{click:true})
        })
    },
    updated(){
      this.scroll.refresh()
    },
    methods:{
      togglePopShow(){
        if (this.totalCount===0){
          return
        }else {
          this.popShow=!this.popShow
        }
          if (!this.scroll){
            this.scroll = new BScroller(this.$refs.foodWrepper,{click:true})
          }else{
            this.scroll.refresh()
          }

      },
      clearFoodList(){
        //利用子组件向父组件传值
        this.$emit("clear-foods")
        this.popShow=false

      }
    },
    props: {
      selectFoods:{
        type:Array,
        default(){
          return []
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice:{
        type:Number,
        default:0
      }
    },
    computed:{
      totalPrice(){
        let price=0;
        this.selectFoods.forEach((food)=>{
          price += food.price * food.count
        })
        return price
      },
      totalCount(){
        let count=0;
        this.selectFoods.forEach((food)=>{
          count += food.count
        })
        return count
      },
      payContent(){
        if (this.selectFoods.length===0){
          return `￥${this.minPrice}起送`

        }else if(this.totalPrice<this.minPrice){
          return '还差'+(this.minPrice-this.totalPrice)+'元起送'
        }else{
          return `去支付`
        }
      },
      payClass(){
        if (this.totalPrice < this.minPrice){
          return 'no-pay'
        }else{
          return 'pay'
        }
      },
      blue(){
        if(this.totalPrice>0){
          return 'yes'
        }else{
          return 'no'
        }
      },
      light(){
        if (this.totalPrice>0){
          return 'yes'
        } else{
          return 'no'
        }
      }
    }
  }
</script>

<style lang="stylus" scoped>
  @import "../../common/stylus/mixin.styl"
  .shopcart
    position fixed
    left 0
    bottom 0
    width 100%
    height 48px
    .content
      display flex
      background-color #141d27
      font-size 0
      .content-left
        flex 1
        .logo-wrapper
          display inline-block
          position relative
          top -10px
          width 44px
          height 44px
          background-color #141d27
          border-radius 50%
          margin-left 12px
          padding 6px
          .logo
            width 100%
            height 100%
            border-radius 50%
            text-align center
            background-color rgba(100 100 100 0.4)
            &.yes
              background-color rgb(0 160 220)
            .icon-shopping_cart
              font-size 24px
              line-height 44px
              color rgba(255 255 255 0.4)
              &.cart
                color #ffffff
            .count
              position absolute
              width 24px
              line-height 16px
              font-size 9px
              font-weight 700
              color: #fff
              border-radius 10px
              background-color red
              z-index 999
        .price
          display inline-block
          font-size 16px
          color rgba(255 255 255 0.4)
          font-weight 700
          line-height 24px
          vertical-align bottom
          margin-bottom 22px
          margin-left 10px
          padding-right 12px
          border-right 1px solid rgba(255 255 255 0.1)
          &.yes
            color #ffffff
        .desc
          display inline-block
          font-size 10px
          color rgba(255 255 255 0.4)
          line-height 48px
          vertical-align top
          margin-left 12px

      .content-right
        flex 0 0 105px
        height 48px
        background-color rgba(100 100 100 0.4)
        right 0
        bottom 0
        font-size 12px
        color rgba(255 255 255 0.4)
        font-weight 700
        line-height 48px
        box-sizing border-box
        text-align center
        padding 0 8px
        &.pay
          background-color rgb(0 160 220)
    .aaa
      position fixed
      bottom 48px
      right 0
      width 100%
      height 100%
      width 100%
      height 100%
      background-color rgba(255 255 255 0.8)
      z-index -1
      .pop-wrapper
        position fixed
        bottom 48px
        right 0
        width 100%
        height 100%
        background-color rgba(7 17 27 0.6)
        backdrop-filter blur(10px)
        .foods-wrapper
          position absolute
          bottom 0
          background-color #ffff
          right 0
          max-height 282px
          width 100%
          .title
            display flex
            width 100%
            height 40px
            background-color #f3f5f7
            padding 0 18px
            box-sizing border-box
            .cart
              flex 1
              font-size 14px
              font-weight 700
              line-height 40px
              color rgb(7 17 27)
            .clear
              flex 1
              font-size 12px
              line-height 40px
              color rgb(0 160 220)
              text-align right
          .food-content
            overflow hidden
            max-height 242px
            .foods
              width 100%
              box-sizing border-box
              padding 0 18px
              .food
                display flex
                height 48px
                font-size 14px
                color rgb(7 17 27)
                line-height 48px
                border-1px(rgba(7 17 27 0.1))
                &.last-child
                  border-none()
                .name
                  flex 1
                  line-height 48px
                .right
                  flex 0 0 119px
                  line-height 48px
                  .price
                    display inline-block
                    font-size 10px
                    line-height 24px
                    font-weight 700
                    color rgb(240 20 20)
                    flex 0 0 10px
                    vertical-align top
                    margin-top 12px
                  .cartcontrol-wrapper
                    box-sizing border-box
                    display inline-block
                    line-height 24px
                    margin-left 12px
</style>
