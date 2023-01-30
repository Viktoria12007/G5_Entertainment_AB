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
</style>
