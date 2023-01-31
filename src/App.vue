<template lang="pug">
  div(id="app")
    MainHeader(@forcedUpdateProducts="forcedUpdateProducts")
    main.main__wrap
      BannerSlider
      ProductsList(:forcedProducts="products")
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
  },
  data () {
    return {
     products: []
    }
  },
  methods: {
    forcedUpdateProducts (value) {
      this.products = value;
    }
  }
}
</script>

<style lang="sass">
.main__wrap
  padding: 8px 0px
@media (min-width: 677px) and (max-width: 834px)
  .main__wrap
    padding: 16px 0px
@media (min-width: 1024px) and (max-width: 1280px)
  .main__wrap
    padding: 16px 0px
@media (min-width: 1440px) and (max-width: 1640px)
  .main__wrap
    padding: 16px 0px
@media (min-width: 1641px)
  .main__wrap
    padding: 16px 0px
</style>
