<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul class="menu">
        <li class="menu-item" v-for="(item,index) in goods " @click="selectMenu(index)"
            :class="{'current':currentIndex === index}">
          <div class="text">
            <span class="icon" v-if="item.type > 0" :class="classMap[item.type]"></span>{{item.name}}
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="good in goods" class="food-list-hook">
          <h1 class="title">{{good.name}}</h1>
          <ul class="foods">
            <li v-for="food in good.foods" class="food">
              <div class="count-wrapper border-1px">
                <div class="avatar">
                  <img :src="food.icon" width="57" height="57">
                </div>
                <div class="content">
                  <p calss="name">{{food.name}}</p>
                  <div class="description">{{food.description}}</div>
                  <div class="count-rating">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span class="rating">好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="new"><span font-size="10px">￥</span>{{food.price}}</span>
                    <span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"
              :selectFoods="selectFoods" @clear-foods="clearFoods()"></shopcart>
  </div>
</template>


<script>
  import BScroller from 'better-scroll'
  import shopcart from '../shopcart/shopcart'
  import cartcontrol from '../cartcontrol/cartcontrol'

  const ERR_OK = 0
  export default {
    name: "goods",
    data() {
      return {
        goods: [],
        classMap: [],
        scrollY: 0,
        listHeight: [],
      }
    },
    props:{
      seller :{}
    },
    components:{
      shopcart,
      cartcontrol
    },
    computed: {
      currentIndex() {
        //判断scrollY在listHeight哪一个区间内
        for (let i = 0; i < this.listHeight.length - 1; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (this.scrollY >= height1 && this.scrollY < height2) {
            return i;
          }
        }
        return 0;
    },
      selectFoods () {
        let foods=[]
        this.goods.forEach((good) => {
          good.foods.forEach((food)=>{
            if (food.count > 0) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      this.$axios.get('/api/goods').then((data) => {
        if (data.data.errno === ERR_OK) {
          this.goods = data.data.data
          this.$nextTick(() => {
            this._initBscroller()
            this._calculateHeight()
          })
        }
      })
    },
    methods: {
      _initBscroller() {
        this.menuScroll = new BScroller(this.$refs.menuWrapper, {click: true})
        //设置监控probeType
        this.foodsScroll = new BScroller(this.$refs.foodsWrapper, {click: true, probeType: 3})
        this.foodsScroll.on("scroll", (pos) => {
          this.scrollY = Math.round(Math.abs(pos.y))
        })
      },
      _calculateHeight() {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook")
        console.log(foodList.length)
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      },
      selectMenu(index) {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook")
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el, 300)
      },
      clearFoods(){
        this.goods.forEach((good) => {
          good.foods.forEach((food)=>{
            if (food.count) {
              food.count = 0
            }
          })
        })
      }
    },
  }
</script>

<style lang="stylus" scoped>
  @import "../../common/stylus/mixin.styl"
  .goods
    display flex
    position absolute
    top 174px
    left 0
    width 100%
    bottom 46px
    .menu-wrapper
      flex 0 0 80px
      background-color #f3f5f7
      overflow hidden
      .menu
        .menu-item
          box-sizing border-box
          display table
          font-size 12px
          width 80px
          height 54px
          line-height 14px
          padding 0 12px
          color rgb(77 85 93)
          &.current
            background-color #fff
            .text
              border-none()
          &:last-child
            border-none()
          .text
            display table-cell
            vertical-align middle
            border-1px(rgba(7 17 27 0.1))
            .icon
              display inline-block
              height 12px
              width 12px
              background-size 12px 12px
              &.decrease
                bg-image('decrease_1')
              &.discount
                bg-image('discount_1')
              &.special
                bg-image('special_1')
              &.invoice
                bg-image('invoice_1')
              &.guarantee
                bg-image('guarantee_1')
    .foods-wrapper
      flex 1
      overflow hidden
      .title
        height 26px
        width 100%
        padding-left 14px
        box-sizing border-box
        line-height 26px
        font-size 12px
        color rgb(147 153 195)
        background-color #f3f5f7
        border-left #d9dde1 solid 3px
      .foods
        .food
          box-sizing border-box
          padding 18px 18px 0 18px
          .count-wrapper
            display flex
            border-1px(rgba(7 17 27 0.1))
            padding-bottom 18px
            .avatar
              flex 0 0 12px
              height 57px
            .content
              flex 1
              margin-left 10px
              .name
                margin-top 2px
                font-size 14px
                line-height 14px
                color rgb(7 17 27)
              .description, .count-rating
                margin-top 8px
                font-size 10px
                color rgb(147 153 159)
                line-height 10px
            .cartcontrol
                position absolute
                bottom 18px
                right 0
          .price
            margin-top 2px
            .new
              font-size 14px
              color rgb(240 20 20)
            .old
              font-size 10px
              color rgb(147 153 159)
              font-weight 700
              text-decoration line-through
</style>
