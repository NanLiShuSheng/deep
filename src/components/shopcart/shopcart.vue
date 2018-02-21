<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="shopcart-left">
        <div class="icon-box">
          <div class="icon" :class="{'active': priceCount>0}">
            <i class="fa fa-shopping-cart"></i>
          </div>
          <div class="num" v-show="goodsCount>0">{{goodsCount}}</div>
        </div>
        <div class="price" :class="{'active': priceCount>0}">￥{{priceCount}}</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="shopcart-right">
        <div class="pay" :class="payClass">{{payPrice}}</div>
      </div>
    </div>
    <div class="ball-container">
      <template v-for="ball in balls">
        <transition name="drap" @before-enter="beforeEnter" @enter="enterIn" @after-enter="afterEnter">
          <div v-show="ball.show" class="ball">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </template>
    </div>
    <transition name="fold">
      <div class="foods-list" v-show="isFold">
        <div class="title">
          <h1 class="cart">购物车</h1>
          <span class="empty">清空</span>
        </div>
        <div class="foods">
          <ul>
            <li v-for="item in selectFoods">
              <div class="food">{{item.name}}</div>
              <div class="price">
                <span>￥{{item.price}}</span>
              </div>
              <div class="cart-wrapper">
                <v-cartcontrol :food="item"></v-cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>
<script type="text/ecmascript-6">
  import cartcontrol from "../cartcontrol/cartcontrol";

  export default {
      props : {
        selectFoods : {
          type : Array,
          default() {
            return []
          }
        },
        deliveryPrice : {
          type : Number,
          default : 0
        },
        minPrice : {
          type : Number,
          default : 0
        }
      },
      data() {
        return {
          balls : [{
            show : false
          },{
            show : false
          },{
            show : false
          },{
            show : false
          },{
            show : false
          }],
          dropballs : [],
          fold: true
        }
      },
      methods : {
        drop(el) {
            for(let i=0;i<this.balls.length;i++) {
              let ball = this.balls[i];
              if(!ball.show) {
                ball.show = true;
                ball.el = el;
                this.dropballs.push(ball);
                return;
              }
            }
        },
        toggleList() {
          if(!this.goodsCount) {
            return;
          }
          this.fold = !this.fold;
        },
        beforeEnter(el) {
          let count = this.balls.length;
          while(count--) {
            let ball = this.balls[count];
            if(ball.show) {
              let rect = ball.el.getBoundingClientRect();
              let x = rect.left - 32;
              let y = -(window.innerHeight - rect.top - 22);
              el.style.webkitTransform = `translate3d(0,${y}px,0)`;
              el.style.transform = `translate3d(0,${y}px,0)`;
              let inner = el.getElementsByClassName('inner-hook')[0];
              inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
              inner.style.transform = `translate3d(${x}px,0,0)`;
            }
          }
        },
        enterIn(el) {
          /* eslint-disable no-unused-vars */
          let rf = el.offsetHeight;
          this.$nextTick(() => {
            el.style.webkitTransform = 'translate3d(0,0,0)';
            el.style.transform = 'translate3d(0,0,0)';
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTransform = 'translate3d(0,0,0)';
            inner.style.transform = 'translate3d(0,0,0)';
          })
        },
        afterEnter(el) {
            let ball = this.dropballs.shift();
            if(ball) {
              ball.show = false;
              el.style.display = 'none';
            }
        }
      },
      computed : {
          priceCount() {
            let price = 0;
            this.selectFoods.forEach((food) => {
                price += food.price * food.count
            });
            return price;
          },
          goodsCount() {
            let count = 0;
            this.selectFoods.forEach((food) => {
              count += food.count;
            });
            return count;
          },
          payPrice() {
            if(this.priceCount === 0) {
              return `￥${this.minPrice}元起送`
            }else if(this.priceCount < this.minPrice) {
              let dull = this.minPrice - this.priceCount;
              return `还需￥${dull}元配送`
            }else {
              return "去结算"
            }
          },
          payClass() {
            if(this.minPrice > this.priceCount) {
              return "no-enough"
            }else {
              return "enough"
            }
          },
          isFold() {
            if(!this.goodsCount) {
              this.fold = true;
              return false;
            }else {
              let show = !this.fold;
              return show;
            }
          }
      },
      components: {
        'v-cartcontrol' : cartcontrol
      }
  };
</script>
<style lang="stylus" ref="stylesheet/stylus">

  .shopcart
    position: fixed
    bottom: 0
    left: 0
    width: 100%
    z-index: 200
    height: 48px
    .content
      display: flex
      background: #141d27
      font-size: 0
      .shopcart-left
        flex: 1
        .icon-box
          display: inline-block
          position: relative
          top: -10px
          margin: 0 12px
          padding: 6px
          height: 56px
          width: 56px
          box-sizing: border-box
          background: #141d27
          border-radius: 50%
          i
            width: 20px
            height: 20px
            display: block
          .icon
            display: inline-block
            width: 100%
            height: 100%
            background: #2b343c
            border-radius: 50%
            &.active
              background: rgb(0,160,220)
          .num
            position: absolute
            top: 0
            right: 0
            height: 16px
            width: 24px
            background: rgb(240,20,20)
            line-height: 16px
            font-size: 9px
            border-radius: 16px
            box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
            color: #fff
            text-align: center
        .price
          display: inline-block
          color: rgba(255,255,255,0.4)
          font-size: 16px
          vertical-align: top
          font-weight: 700
          line-height: 24px
          margin-top: 12px
          border-right: 1px solid rgba(255,255,255,0.1)
          padding-right: 12px
          box-sizing: border-box
          &.active
            color:#fff
        .desc
          display: inline-block
          font-size: 16px
          line-height: 24px
          vertical-align: top
          color: rgba(255,255,255,0.4)
          margin-left: 12px
          margin-top: 12px
      .shopcart-right
        flex: 0,0,105px
        width: 105px
        .pay
          height: 48px
          line-height: 48px
          font-size: 12px
          font-weight: 700
          text-align: center
          background: #2b333b
          &.no-enough
            color: rgba(255,255,255,0.4)
          &.enough
            background: rgb(0,160,220)
            color: #fff
    .ball-container
      .ball
        position: fixed
        left: 32px
        bottom : 22px
        z-index: 66666
        &.drap-enter-active,&.drap-leave-active
          transition: all .5s cubic-bezier(0.49,-0.29,0.75,0.41)
          .inner
            height: 16px
            width: 16px
            border-radius: 50%
            background: rgb(0,160,220)
            transition: all .5s linear
    .foods-list
      position: absolute
      top: 0
      left: 0
      z-index: -1
      width: 100%
      transform: translate3d(0,-100%,0)
      &.fold-enter-active, &.fold-leave-active
        transition: all .5s linear
      &.fold-enter, &.fold-leave-to
        transform: translate3d(0,0,0)
      .title
        height: 40px
        line-height: 40px
        background: #f3f5f7
        padding: 0 18px
        .cart
          float: left
          font-size:14px
          color: rgb(7,17,27)
        .empty
          float: right
          color: rgb(0,160,200)
          font-size: 14px
      .foods
        position: relative
        padding: 0 18px
        background: #fff
        max-height: 271px
        overflow: hidden
        width:100%
        box-sizing: border-box
        li
          display: block
          position: relative
          padding: 12px 0
          .name
            font-weight: bold
            color: rgb(7,17,27)
            font-size: 14px
          .price
            font-weight: bold
            color: rgb(240,20,20)
            font-size: 14px
            position:absolute
            bottom: 12px
            right: 100px
          .cart-wrapper
            position:absolute
            right: 0
            bottom: 6px
</style>
