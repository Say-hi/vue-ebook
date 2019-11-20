<template>
  <div class='flap-card-wrapper'
       v-show='flapCardVisible'>
    <div class='flap-card-bg' v-show='runFlapCardAnimation' :class='{"animation": runFlapCardAnimation}'>
      <div class='flap-card'
           :style='{zIndex: item.zIndex}'
           v-for='(item, index) in flapCardList'
           :key='index'>
        <div class='flap-car-circle'>
          <div class='flap-card-semi-circle flap-card-semi-circle-left'
               ref='left'
               :style='semiCircleStyle(item, "left")'></div>
          <div class='flap-card-semi-circle flap-card-semi-circle-right'
               ref='right'
               :style='semiCircleStyle(item, "right")'></div>
        </div>
      </div>
      <div class='point-wrapper'>
        <div class='point'
             :class='{"animation": runPointAnimation}'
             v-for='item in pointList'
             :key='item'></div>
      </div>
    </div>
    <div class='book-card' v-show='runBookCardAnimation' :class='{"animation": runBookCardAnimation}'>
      <div class='book-card-wrapper'>
        <div class='img-wrapper'>
          <div class='img-wrapper'>
            <img class='img' :src='data ? data.cover : ""' alt=''>
          </div>
        </div>
        <div class='content-wrapper'>
          <div class='content-title'>{{data ? data.title:''}}</div>
          <div class='content-author sub-title-medium'>{{data?data.author:''}}</div>
          <div class='content-category'>{{categoryText()}}</div>
        </div>
        <div class='read-btn' @click.stop='showBookDetail(data)'>{{$t('home.readNow')}}</div>
      </div>
    </div>
    <div class='close-btn-wrapper'
         @click='close'>
      <div class='icon-close'></div>
    </div>
  </div>
</template>

<script>
  import {storeHomeMixin} from '../../utils/mixin'
  import {flapCardList, categoryText} from '../../utils/store'
  export default {
    mixins: [storeHomeMixin],
    props: {
      data: Object
    },
    data() {
      return {
        flapCardList,
        front: 0,
        back: 1,
        intervalTime: 20,
        runFlapCardAnimation: false,
        pointList: null,
        runPointAnimation: false,
        runBookCardAnimation: false
      }
    },
    created () {
      this.pointList = []
      for (let i = 0; i < 18; i++) {
        this.pointList.push(`point${i}`)
      }
    },
    methods: {
      categoryText () {
        if (this.data) {
          categoryText(this.data.category, this)
        }
      },
      close () {
        this.setFlapCardVisible(false)
        this.stopAnimation()
      },
      semiCircleStyle(item, dir) {
        return {
          backgroundColor: `rgb(${item.r}, ${item.g}, ${item.b})`,
          backgroundSize: item.backgroundSize,
          backgroundImage: dir === 'left' ? item.imgLeft : item.imgRight
        }
      },
      rotate(index, type) {
        const item = this.flapCardList[index]
        let dom
        if (type === 'front') {
          dom = this.$refs.right[index]
        } else {
          dom = this.$refs.left[index]
        }
        dom.style.transform = `rotateY(${item.rotateDegree}deg)`
        dom.style.backgroundColor = `rgb(${item.r}, ${item._g}, ${item.b})`
      },
      flapCardRotate () {
        const frontFlapCard = this.flapCardList[this.front]
        const backFlapCard = this.flapCardList[this.back]
        frontFlapCard.rotateDegree += 10
        frontFlapCard._g -= 5
        backFlapCard.rotateDegree -= 10
        if (backFlapCard.rotateDegree < 90) {
          backFlapCard._g += 5
        }
        if (frontFlapCard.rotateDegree === 90 && backFlapCard.rotateDegree === 90) {
          backFlapCard.zIndex += 2
        }
        this.rotate(this.front, 'front')
        this.rotate(this.back, 'back')
        if (frontFlapCard.rotateDegree === 180 && backFlapCard.rotateDegree === 0) {
          this.next()
        }
      },
      next() {
        const frontFlapCard = this.flapCardList[this.front]
        const backFlapCard = this.flapCardList[this.back]
        frontFlapCard.rotateDegree = 0
        backFlapCard.rotateDegree = 0
        frontFlapCard._g = frontFlapCard.g
        backFlapCard._g = backFlapCard.g
        this.rotate(this.front, 'front')
        this.rotate(this.back, 'back')
        this.front++
        this.back++
        const len = this.flapCardList.length
        if (this.front >= len) {
          this.front = 0
        }
        if (this.back >= len) {
          this.back = 0
        }
        this.flapCardList.forEach((item, index) => {
          item.zIndex = 100 - ((index - this.front + len) % len)
        })
        this.prepare()
      },
      prepare() {
        const backFlapCard = this.flapCardList[this.back]
        backFlapCard.rotateDegree = 180
        backFlapCard._g = backFlapCard.g - 5 * 9
        this.rotate(this.back, 'back')
      },
      startFlapCardAnimation () {
        this.prepare()
        this.task = setInterval(() => {
          this.flapCardRotate()
        }, this.intervalTime)
      },
      startPointAnimation () {
        this.runPointAnimation = true
        setTimeout(() => {
          this.runPointAnimation = false
        }, 750)
      },
      reset() {
        this.front = 0
        this.back = 1
        this.flapCardList.forEach((item, index) => {
          item.zIndex = 100 - index
          item._g = item.g
          item.rotateDegree = 0
          this.rotate(index, 'front')
          this.rotate(index, 'back')
        })
        this.runBookCardAnimation = false
        this.runFlapCardAnimation = false
        this.runPointAnimation = false
      },
      stopAnimation() {
        if (this.task) {
          clearInterval(this.task)
        }
        if (this.timeout) {
          clearInterval(this.timeout)
        }
        if (this.timeout2) {
          clearInterval(this.timeout2)
        }
        this.reset()
      },
      runAnimation () {
        this.runFlapCardAnimation = true
        this.timeout = setTimeout(() => {
          this.startFlapCardAnimation()
          this.startPointAnimation()
        }, 300)
        this.timeout2 = setTimeout(() => {
          this.stopAnimation()
          this.runBookCardAnimation = true
        }, 2500)
      }
    },
    mounted() {
      console.log(this.data)
      // this.startFlapCardAnimation()
    },
    watch: {
      flapCardVisible (v) {
        if (v) {
          this.runAnimation()
        }
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../assets/styles/global";
  @import "../../assets/styles/flapCard";
  .flap-card-wrapper {
    z-index: 1000;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.6);
    @include center;
    @include absCenter;
    .flap-card-bg {
      position: relative;
      width: px2rem(64);
      height: px2rem(64);
      border-radius: px2rem(5);
      background: #fff;
      transform: scale(0);
      opacity: 0;
      &.animation {
        animation: flap-card-move .3s ease-in forwards;
      }
      @keyframes flap-card-move {
        0% {
          transform: scale(0);
          opacity: 0;
        }
        50% {
          transform: scale(1.2);
          opacity: 1;
        }
        75% {
          transform: scale(0.9);
          opacity: 1;
        }
        100% {
          transform: scale(1);
          opacity: 1;
        }
      }
      .flap-card {
        width: px2rem(48);
        height: px2rem(48);
        @include absCenter;
        .flap-car-circle {
          width: 100%;
          height: 100%;
          display: flex;
          .flap-card-semi-circle {
            flex: 0 0 50%;
            width: 50%;
            height: 100%;
            background-repeat: no-repeat;
            backface-visibility: hidden;
          }
          .flap-card-semi-circle-left {
            border-radius: px2rem(24) 0 0 px2rem(24);
            background-position: center right;
            transform-origin: right;
          }
          .flap-card-semi-circle-right {
            border-radius: 0 px2rem(24) px2rem(24) 0;
            background-position: center left;
            transform-origin: left;
          }
        }
      }
      .point-wrapper {
        z-index: 1500;
        @include absCenter;
        .point {
          border-radius: 50%;
          @include absCenter;
          &.animation {
            @for $i from 1 to length($moves) {
              &:nth-child(#{$i}) {
                @include move($i);
              }
            }
          }
        }
      }
    }
    .book-card {
      position: relative;
      width: 65%;
      max-width: px2rem(400);
      border-radius: px2rem(15);
      background: #fff;
      &.animation {
        animation: scale .3s ease-in forwards;
        @keyframes scale {
          0% {
            transform: scale(0);
            opacity: 0;
          }
          100% {
            transform: scale(1);
            opacity: 1;
          }
        }
      }
      .book-card-wrapper {
        width: 100%;
        height: 100%;
        margin-bottom: px2rem(40);
        @include columnTop;
        .img-wrapper {
          width: 100%;
          margin-top: px2rem(20);
          @include center;
          .img {
            width: px2rem(90);
            height: px2rem(130);
          }
        }
        .content-wrapper {
          padding: 0 px2rem(20);
          margin-top: px2rem(20);
          .content-title {
            color: #333;
            font-weight: bold;
            font-size: px2rem(18);
            line-height: px2rem(20);
            margin-top: px2rem(10);
            text-align: center;
            @include ellipsis2(2)
          }
          .content-author {
            margin-top: px2rem(10);
            text-align: center;
          }
          .content-category {
            color: #999;
            font-size: px2rem(14);
            margin-top: px2rem(10);
            text-align: center;
          }
        }
        .read-btn {
          position: absolute;
          bottom: 0;
          left: 0;
          z-index: 1100;
          width: 100%;
          border-radius: 0 0 px2rem(15) px2rem(15);
          padding: px2rem(15) 0;
          text-align: center;
          color: white;
          font-size: px2rem(14);
          background: $color-blue;
        }
      }
    }
    .close-btn-wrapper {
      position: absolute;
      left: 0;
      bottom: px2rem(30);
      z-index: 1100;
      width: 100%;
      @include center;
      .icon-close {
        width: px2rem(45);
        height: px2rem(45);
        font-size: px2rem(25);
        border-radius: 50%;
        background: #333;
        color: #fff;
        @include center;
      }
    }
  }
</style>
