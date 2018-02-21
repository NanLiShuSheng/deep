<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="btn-decrease" v-show="food.count>0" @click="decr"></div>
    </transition>
    <div class="count" v-show="food.count>0">{{food.count}}</div>
    <div class="btn-add" @click.stop.prevent="addNum($event)"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props : {
      food : {
        type : Object
      }
    },
    created() {

    },
    methods : {
      addNum(event) {
        if(!this.food.count) {
            Vue.set(this.food,'count',1)
        }else {
          this.food.count ++ ;
        };
        this.$emit('cart-add',event.target);
      },
      decr() {
        this.food.count --;
      }
    }
  };
</script>
<style lang="stylus" ref="stylesheet/stylus">

  .move-enter-active,.move-leave-active
    opacity: 1
    transition: all .5s linear
    transform: translate3D(0,0,0)
  .move-leave-to,.move-enter
    opacity: 0
    transform: translate3D(24px,0,0)
  .cartcontrol
    font-size: 0
    .btn-decrease,.btn-add
      display: inline-block
      height: 24px
      width: 24px
      border-radius: 50%;
      border: 2px solid rgb(0,160,220)
      box-sizing: border-box
    .count
      display: inline-block
      font-size: 16px
      vertical-align : top
      padding-top: 6px
      width: 24px
      text-align: center
    .btn-add
      display: inline-block
      background: rgb(0,160,220)
</style>
