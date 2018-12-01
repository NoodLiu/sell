<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" height="64" width="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="brand" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
        <div class="supports-count" v-if="seller.supports">
          <span :class="" class="count" @click="showDetail=true">{{seller.supports.length}}个</span>
          <span class="icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <div class="title">
      </div>
      <div class="text">{{seller.bulletin}}</div>
      <div class="icon">
        <span class="icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="200%">
    </div>
    <div class="detail" v-show="showDetail">
        <div class="detail-wrapper">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper"></div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul class="supports">
              <li v-for="support in seller.supports" class="support-item">
                <span class="icon" :class="classMap[support.type]"></span>
                <span class="text">{{support.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家优惠</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              {{seller.bulletin}}
            </div>
          </div>
          <div class="close">
            <span class="icon-close" @click="showDetail = false"></span>
          </div>
        </div>

    </div>
  </div>
</template>

<script>
  export default {
    name: "header",
    data() {
      classMap:{}
      return {
        showDetail: false
      }
    },
    props: {
      seller: {
      }
    },
    created() {
      this.classMap = ['decrease','discount','special','invoice','guarantee']
    }
  }
</script>

<style lang="stylus" scoped>
  @import "../../common/stylus/mixin.styl"

  .header
    position relative
    overflow hidden
    background-color rgba(7,17,27,0.5)
    color: #ffffff
    .content-wrapper
      position: relative
      display: flex
      height 64px
      padding 24px 12px 18px 12px
      .avatar
        width 64px
      .content
        margin-left 16px
        .title
          margin-top 2px
          margin-bottom 8px
          .brand
            font-size 0
            display inline-block
            width 30px
            height 18px
            bg-image('brand')
            background-size 30px 18px
          .name
            margin-left 6px
            font-size 16px
            font-weight bold
            line-height 18px
            vertical-align top
        .description
          font-size 12px
          font-weight 200
          line-height 12px
          margin-bottom 10px
        .support
          margin-bottom 2px
          .brand
            display inline-block
            width 12px
            height 12px
            background-size 12px 12px
            vertical-align top
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
          .text
            font-size 10px
            font-weight 200
            line-height 12px
            vertical-align top
      .supports-count
        position absolute
        right 12px
        bottom 6px
        background-color: rgba(0,0,0,0.2)
        padding 7px 8px
        border-radius 14px
        font-size 0
        .count
          font-size 10px
          font-weight 200
          line-height 12px
        .icon-keyboard_arrow_right
          font-size 12px
          margin-left 3px
    .bulletin-wrapper
      position relative
      height 28px
      padding 0 12px
      background-color rgba(7,17,27,0.2)
      display flex
      .title
        flex 0 0 22px
        height 100%
        width 22px
        margin-top 8px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat no-repeat
      .text
        width 90%
        height 28px
        white-space nowrap
        overflow:hidden;
        text-overflow:ellipsis;
        font-size 10px
        font-weight 200
        line-height 28px
        margin-left 4px
      .icon
        flex 1
        position absolute
        right 12px
        margin-left 4px
        top 5px
  .background
    position absolute
    left 0
    top 0
    z-index -1
    width 100%
    height 100%
    filter blur(10px)
  .detail
    position fixed
    z-index 100
    top 0
    left 0
    width 100%
    height 100%
    background-color rgba(7 17 27 0.8)
    backdrop-filter blur(10px)
    overflow auto
    .detail-wrapper
      width 80%
      min-height 80%
      margin 0 auto
      position relative
      padding-bottom 64px
      .detail-main
        margin-top 64px
        .name
          font-size 16px
          line-height 16px
          font-weight 700
          color #ffffff
          display block
          text-align center
        .star-wrapper
          height 24px
          margin-top 16px
        .title
          margin-top 28px
          font-size 0
          .line
            display inline-block
            border-bottom 1px solid rgba(255 255 255 0.2)
            width 34%
            margin-bottom 5px
          .text
            margin 0 12px
            display inline-block
            font-size 14px
            line-height 14px
            font-weight 700
        .supports
          .support-item
            font-size 0
            margin-top 12px
            margin-left 12px
            &:first-child
             margin-top 24px
            .icon
              vertical-align top
              display inline-block
              width 16px
              height 16px
              background-size 16px 16px
              &.decrease
                bg-image('decrease_2')
              &.discount
                bg-image('discount_2')
              &.special
                bg-image('special_2')
              &.invoice
                bg-image('invoice_2')
              &.guarantee
                bg-image('guarantee_2')
            .text
              vertical-align top
              font-size 12px
              line-height 12px
              font-weight 200
              color #ffffff
              margin-left 6px
        .bulletin
          margin-top 24px
          padding 0 12px
          font-size 12px
          font-weight 200
          line-height 24px
      .close
        position absolute
        right 0
        bottom 0
        width 100%
        margin-bottom 32px
        text-align center
        .icon-close
          font-size 32px
          color rgba(255 255 255 0.5)
</style>
