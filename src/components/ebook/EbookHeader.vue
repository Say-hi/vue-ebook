<template>
  <div class='ebook-header'>
    <div class='ebook-header-text'>{{getSectionName}}</div>
  </div>
</template>

<script>
  import {ebookMixin} from '../../utils/mixin'

  export default {
    mixins: [ebookMixin]
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../assets/styles/global";
  .ebook-header {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    .ebook-header-text {
      font-size: px2rem(14);
      line-height: px2rem(16);
      padding: px2rem(10);
      box-sizing: border-box;
      @include ellipsis;
    }
  }
</style>
