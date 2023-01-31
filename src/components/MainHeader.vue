<template lang="pug">
header.header
  div.container.header__container
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
      try {
        const response = await axios.get('https://dummyjson.com/products?limit=100');
        this.updatedProducts = response.data.products;
      } catch (e) {
        console.error(e);
      }
      this.$emit('forcedUpdateProducts', this.updatedProducts);
    },
  },
}
</script>

<style lang="sass">
.header
  min-height: 48px
  background-color: $primary
  &__container
    display: flex
    justify-content: space-between
    align-items: center
    padding: 7px 0
.logo
  &__link
    display: none
  &__img
    width: 28px
    height: 28px
.update-products-button
  display: none
  font-size: 15px
  font-weight: 600
  line-height: 18px
  color: $button-black
  padding: 8px 16px
  border: none
  border-radius: 100px
  background-color: #ffffff
@media (min-width: 834px) and (max-width: 1024px)
  .header
    min-height: auto
  .logo__link
    display: inline-block
  .update-products-button
    display: block
@media (min-width: 1024px) and (max-width: 1280px)
  .header
    min-height: auto
  .logo
    &__link
      display: inline-block
    &__img
      width: 26px
      height: 26px
  .update-products-button
    display: block
@media (min-width: 1280px) and (max-width: 1440px)
  .header
    min-height: auto
  .logo
    &__link
      display: inline-block
    &__img
      width: 24px
      height: 24px
  .update-products-button
    display: block
    font-size: 14px
    line-height: 17px
@media (min-width: 1440px) and (max-width: 1640px)
  .header
    min-height: auto
    &__container
      padding: 8px 0px
  .logo
    &__link
      display: inline-block
    &__img
      width: 22px
      height: 22px
  .update-products-button
    display: block
    font-size: 13px
    line-height: 15.7px
@media (min-width: 1641px)
  .header
    min-height: auto
    &__container
      padding: 8px 0px
  .logo
    &__link
      display: inline-block
    &__img
      width: 20px
      height: 20px
  .update-products-button
    display: block
    font-size: 12px
    line-height: 15px
</style>
