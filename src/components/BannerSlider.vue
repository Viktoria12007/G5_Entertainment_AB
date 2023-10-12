<template lang="pug">
  section.swiper__section
    div.container
      div.swiper__wrap
        div.swiper__loading(v-if="!imgs.length")
          div Loading...
        swiper.swiper-no-swiping(
          else,
          ref="mainSwiper",
          :observer="true",
          :observeParents="true",
          :observeSlideChildren="true",
          :slides-per-view="1",
          :slides-per-group="1",
          :space-between="30",
          :loop="true",
          :pagination="true",
          :navigation="true",
          :breakpoints="breakpoints")
          swiper-slide(v-for="(img, index) in imgs",
            :key="index")
            div.slide__img(:style="{ backgroundImage: `url(${img})` }")
</template>

<script>
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
      breakpoints: {
        769: {
          slidesPerView: 2,
          slidesPerGroup: 2,
        },
        1440: {
          slidesPerView: 3,
          slidesPerGroup: 3,
        },
      },
    }
  },
  async mounted () {
    if (this.getCookie(this.nameCookie)) {
       this.imgs = this.getCookie(this.nameCookie);
    } else {
      while (this.imgs.length < 10) {
        try {
          const response = await axios.get('https://random.dog/woof.json');
          if (['png', 'jpeg', 'jpg', 'gif'].includes(response.data.url.slice(-3).toLowerCase())) {
            this.imgs.push(response.data.url);
          }
        } catch (e) {
          console.error(e);
        }
      }
      this.setCookie(this.nameCookie, this.imgs);
    }
  },
  methods: {
    setCookie (key, value) {
      document.cookie = key + '=' + encodeURIComponent(JSON.stringify(value)) + '; max-age=300';
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
.swiper
  &__section
    margin-bottom: 1px
  &__wrap
    width: 100%
    height: 163px
    overflow: hidden
  &__loading
    width: 100%
    height: 100%
    display: flex
    align-items: center
    justify-content: center
    background-color: $gray
  &-container
    padding-bottom: 17px
  &-slide
    height: 143px
.swiper-container-horizontal > .swiper-pagination-bullets
  bottom: 0px
.swiper-container-horizontal > .swiper-pagination-bullets .swiper-pagination-bullet
  margin: 0 4px
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
  background-position: center center
  background-repeat: no-repeat
  background-size: 7.8px 14.4px
.swiper-button-prev
  left: 4px
  background-image: url("../assets/back.svg")
.swiper-button-next
  right: 4px
  background-image: url("../assets/right.svg")
.swiper-button-prev::after,
.swiper-button-next::after
  display: none
.slide__img
  width: 100%
  height: 100%
  background-repeat: no-repeat
  background-position: 50% 50%
  background-size: cover
@media (min-width: 342px) and (max-width: 430px)
  .swiper
    &__section
      margin-bottom: 28px
    &-container
      padding-bottom: 20px
  .swiper-button-prev
    left: 8px
  .swiper-button-next
    right: 8px
@media (min-width: 430px) and (max-width: 676px)
  .swiper
    &__section
      margin-bottom: 12px
    &__wrap
      height: 151px
    &-container
      padding-bottom: 21px
    &-slide
      height: 130px
  .swiper-button-prev
    left: 8px
  .swiper-button-next
    right: 8px
@media (min-width: 677px) and (max-width: 834px)
  .swiper
    &__section
      margin-bottom: 12px
    &__wrap
      height: 232px
    &-container
      padding-bottom: 25px
    &-slide
      height: 208px
  .swiper-button-prev,
  .swiper-button-next
    width: 32px
    height: 32px
    background-size: 10.4px 19.2px
  .swiper-button-prev
    top: 47%
    left: 7px
  .swiper-button-next
    top: 47%
    right: 7px
  .swiper-pagination-bullet
    width: 12px
    height: 12px
@media (min-width: 834px) and (max-width: 1024px)
  .swiper
    &__section
      margin-bottom: 22px
    &__wrap
      height: 190px
    &-container
      padding-bottom: 25px
    &-slide
      height: 167px
  .swiper-button-prev,
  .swiper-button-next
    width: 32px
    height: 32px
    background-size: 10.4px 19.2px
  .swiper-button-prev
    top: 47%
    left: 7px
  .swiper-button-next
    top: 47%
    right: 7px
  .swiper-pagination-bullet
    width: 12px
    height: 12px
@media (min-width: 1024px) and (max-width: 1280px)
  .swiper
    &__section
      margin-bottom: 12px
    &__wrap
      height: 210px
    &-container
      padding-bottom: 23px
    &-slide
      height: 187px
  .swiper-button-prev,
  .swiper-button-next
    width: 32px
    height: 32px
    background-size: 10.4px 19.2px
  .swiper-button-prev
    top: 47%
    left: 7px
  .swiper-button-next
    top: 47%
    right: 7px
  .swiper-pagination-bullet
    width: 12px
    height: 12px
@media (min-width: 1280px) and (max-width: 1440px)
  .swiper
    &__section
      margin-bottom: 22px
    &__wrap
      height: 273px
    &-container
      padding-bottom: 25px
    &-slide
      height: 250px
  .swiper-button-prev,
  .swiper-button-next
    width: 40px
    height: 40px
    background-size: 13px 24px
  .swiper-button-prev
    top: 47%
    left: 8px
  .swiper-button-next
    top: 47%
    right: 8px
  .swiper-pagination-bullet
    width: 12px
    height: 12px
@media (min-width: 1440px) and (max-width: 1640px)
  .swiper
    &__section
      margin-bottom: 20px
    &__wrap
      height: 297px
    &-container
      padding-bottom: 25px
    &-slide
      height: 272px
  .swiper-button-prev,
  .swiper-button-next
    width: 40px
    height: 40px
    background-size: 13px 24px
  .swiper-button-prev
    top: 47%
    left: 8px
  .swiper-button-next
    top: 47%
    right: 8px
  .swiper-pagination-bullet
    width: 12px
    height: 12px
@media (min-width: 1641px)
  .swiper
    &__section
      margin-bottom: 23px
    &__wrap
      height: 327px
    &-container
      padding-bottom: 33px
    &-slide
      height: 294px
  .swiper-button-prev,
  .swiper-button-next
    width: 48px
    height: 48px
    background-size: 15.6px 28.8px
  .swiper-button-prev
    top: 45%
    left: 16px
  .swiper-button-next
    top: 45%
    right: 16px
  .swiper-pagination-bullet
    width: 12px
    height: 12px
</style>
