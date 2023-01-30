<template lang="pug">
header.header
  div.container
    a.logo__link(href="#")
      img.logo__img(src="../assets/logo.png", alt="Логотип")
    button.update-products-button(@click="updateProducts") Update Products
</template>

<script>
import axios from 'axios'

export default {
  name: 'MainHeader',
  data () {
    return {
      updatedProducts: [],
    }
  },
  methods: {
    async updateProducts () {
      const response = await axios.get('https://dummyjson.com/products?limit=100');
      this.updatedProducts = response.data.products;
      this.$emit('forcedUpdateProducts', this.updatedProducts);
    },
  },
}
</script>

<style lang="sass">
.header
  display: flex
  justify-content: space-between
  height: 48px
  background-color: $primary
.logo__link
  display: none
.update-products-button
  display: none
</style>
