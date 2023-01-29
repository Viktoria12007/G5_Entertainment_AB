<template lang="pug">
  div(id="app")
    MainHeader
    main.main__wrap
      BannerSlider
      ProductsList
</template>

<script>
import MainHeader from './components/MainHeader.vue'
import BannerSlider from '@/components/BannerSlider'
import ProductsList from '@/components/ProductsList'

export default {
  name: 'App',
  components: {
    ProductsList,
    BannerSlider,
    MainHeader
  }
}
</script>

<style lang="sass">
.main__wrap
  padding: 8px 0px
</style>
