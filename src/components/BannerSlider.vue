<template lang="pug">
  section
    div.container
      div.swiper__wrap-loading
        div.swiper__loading(v-if="!imgs.length")
          div Loading...
        swiper.swiper-no-swiping(
          else,
          ref="mainSwiper",
          :observer="true",
          :observeParents="true",
          :observeSlideChildren="true",
          :slides-per-view="1",
          :space-between="30",
          :loop="true",
          :pagination="true",
          :navigation="true")
          swiper-slide(v-for="(img, index) in imgs",
            :key="index")
            div.slide__img(:style="{ backgroundImage: `url(${img})` }")
            //img.slide__img(:src="img")
            //div.slide__wrap
              img.slide__img(:src="img.url"
                blank="true")
</template>

<script>
//:observer="true",
//:observeParents="true",
//:observeSlideChildren="true",
import axios from 'axios'
import { Navigation, Pagination } from 'swiper'
import { SwiperCore, Swiper, SwiperSlide } from 'swiper-vue2'
import 'swiper/swiper-bundle.css'
SwiperCore.use([Navigation, Pagination])

export default {
  name: 'BannerSlider',
  components: {
    Swiper,
    SwiperSlide
  },
  data() {
    return {
      imgs: [],
      nameCookie: 'urlsForSwiper',
    }
  },
  async mounted () {
    if (this.getCookie(this.nameCookie)) {
       this.imgs = this.getCookie(this.nameCookie);
      this.setCookie(this.nameCookie, this.imgs);
    } else {
      while (this.imgs.length < 10) {
        try {
          const response = await axios.get('https://random.dog/woof.json');
          if (['png', 'jpeg', 'jpg', 'gif'].includes(response.data.url.slice(-3).toLowerCase())) {
            this.imgs.push(response.data.url);
          }
        } catch (e) {
          console.debug(e);
        }
      }
      this.setCookie(this.nameCookie, this.imgs);
    }
  },
  methods: {
    setCookie (key, value) {
      // document.cookie = key + '=' + encodeURIComponent(JSON.stringify(value)) + '; max-age=300';
      document.cookie = key + '=' + encodeURIComponent(JSON.stringify(value)) + '; max-age=0';
      // document.cookie = key + '=' + encodeURIComponent(JSON.stringify(value));
    },
    getCookie (key) {
      const matches = document.cookie.match(new RegExp(
        '(?:^|; )' + key.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + '=([^;]*)'
      ));
      return matches ? JSON.parse(decodeURIComponent(matches[1])) : undefined;
    },
  }
}
</script>

<style lang="sass">
//.wrap-swiper
//  padding-top: 8px
//  padding-bottom: 20px
.swiper__wrap-loading
  width: 100%
  height: 163px
  text-align: center
  overflow: hidden
.swiper__loading
  width: 100%
  height: 100%
  display: flex
  align-items: center
  justify-content: center
  background-color: $gray
.swiper-container
  padding-bottom: 20px
//.swiper-wrapper
//  height: 163px
.swiper-slide
 height: 143px
.swiper-container-horizontal > .swiper-pagination-bullets
  bottom: 0px
.swiper-container-horizontal > .swiper-pagination-bullets .swiper-pagination-bullet
  margin: 0 8px
.swiper-pagination-bullet
  width: 8px
  height: 8px
  opacity: 1
  background-color: $gray
.swiper-pagination-bullet-active
  background-color: $primary
.swiper-button-prev,
.swiper-button-next
  width: 24px
  height: 24px
  background-color: rgba(0, 0, 0, 0.5)
  border-radius: 100px
  background-position: center
  background-repeat: no-repeat
  background-size: 7.8px 14.4px
.swiper-button-prev
  background-image: url("../assets/back.svg")
.swiper-button-next
  background-image: url("../assets/right.svg")
.swiper-button-prev::after,
.swiper-button-next::after
  display: none
//.slide__wrap
//  position: relative
//  padding-top: 56.25%
//.slide__img
//  position: absolute
//  left: 0
//  top: 0
//  width: 100%
//  height: auto
.slide__img
  width: 100%
  height: 100%
  background-repeat: no-repeat
  background-position: 50% 50%
  background-size: cover
//.slide__img
//  width: 100%
//  height: 100%
</style>
