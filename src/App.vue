<template>
  <div id="app">
    <router-view />
    <div v-show='!bookAvailable' class='loading'>
      <ebook-loading></ebook-loading>
    </div>
  </div>
</template>

<script>
import EbookLoading from './components/ebook/EbookLoading'
import {ebookMixin} from './utils/mixin'
export default {
  mixins: [ebookMixin],
  components: {
    EbookLoading
  }
}
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html')
  let fontSize = window.innerWidth / 10
  fontSize = fontSize > 50 ? 50 : fontSize
  html.style.fontSize = fontSize + 'px'
})
</script>

<style lang='scss' scoped>
  @import "./assets/styles/global";
  #app {
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #333333;
  }
  .loading {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    @include center;
  }
</style>>
