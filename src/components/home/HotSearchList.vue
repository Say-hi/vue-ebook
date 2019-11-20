<template>
  <transition name='hot-search-move'>
    <scroll class='hot-search-wrapper'
            :top='52'
            @onScroll='onScroll'
            ref='scroll'
    >
      <hot-search :label='$t("home.hotSearch")'
                  :btn='$t("home.change")'
                  :hotSearch='searchList.hotSearch'></hot-search>
      <div class='line'></div>
      <hot-search :label='$t("home.historySearch")'
                  :btn='$t("home.clear")'
                  :hotSearch='searchList.historySearch'></hot-search>
    </scroll>
  </transition>
</template>

<script>
  import Scroll from '../common/Scroll'
  import HotSearch from './HotSearch'
  import {storeHomeMixin} from '../../utils/mixin'
  export default {
    data () {
      return {
        searchList: {
          hotSearch: [
            {
              type: 1,
              text: '热门搜索',
              num: 123
            },
            {
              type: 1,
              text: '热门搜索',
              num: 123
            }
          ],
          historySearch: [
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            },
            {
              type: 2,
              text: '热门搜索'
            }
          ]
        }
      }
    },
    mixins: [
      storeHomeMixin
    ],
    components: {
      Scroll,
      HotSearch
    },
    methods: {
      onScroll (offsetY) {
        this.setHotSearchOffsetY(offsetY)
      },
      reset () {
        this.$refs.scroll.scrollTo(0, 0)
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../assets/styles/global";
</style>
