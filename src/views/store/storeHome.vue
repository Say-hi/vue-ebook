<template>
  <div class='store-home'>
    <flab-card :data='random'></flab-card>
    <search-bar></search-bar>
    <scroll :top='scrollTop' @onScroll='onScroll' ref='scroll'>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
      <div>13216541654</div>
    </scroll>
  </div>
</template>

<script>
  import SearchBar from '../../components/home/SearchBar'
  import Scroll from '../../components/common/Scroll'
  import FlabCard from '../../components/home/FlapCard'
  import { storeHomeMixin } from '../../utils/mixin'
  import {home} from '../../api/store'
  export default {
    mixins: [storeHomeMixin],
    mounted () {
      home().then(res => {
        if (res && res.status === 200) {
          const data = res.data
          const randomIndex = Math.floor(Math.random() * data.random.length)
          this.random = data.random[randomIndex]
        }
      })
    },
    components: {
      SearchBar,
      Scroll,
      FlabCard
    },
    data () {
      return {
        random: null,
        scrollTop: 94
      }
    },
    methods: {
      onScroll (offsetY) {
        this.setOffsetY(offsetY)
        if (offsetY > 0) {
          this.scrollTop = 52
        } else {
          this.scrollTop = 94
        }
        this.$refs.scroll.refresh()
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../assets/styles/global";
  .store-home {
    width: 100%;
    height: 100%;
  }
</style>
