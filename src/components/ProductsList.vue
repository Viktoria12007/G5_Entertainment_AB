<template lang="pug">
  section.products-list__section
    div.container
      div.products-list
        div.product-item.products-list__item(
          v-for="(product, index) in products",
          :key="product.id")
          div.product-item__wrap-img
            img.product-item__img(:src="product.thumbnail", loading="lazy", :alt="product.title")
            div.product-item__badge(
              v-if="index < 3",
              :class="getBadge(index).class",
              ) {{ getBadge(index).text }}
          div.product-item__row
            span.product-item__title {{ 'Brand: ' }}
            span.product-item__text {{ product.brand }}
          div.product-item__row
            span.product-item__title {{ 'Category: ' }}
            span.product-item__text {{ product.category }}
          div.product-item__row.product-item__row_description
            span.product-item__text.product-item__multi-text
              span.product-item__title {{ 'Description: ' }}
              span {{ product.description }}
          div.product-item__row
            span.product-item__title {{ 'In stock: ' }}
            span.product-item__text {{ product.stock }}
</template>

<script>
import axios from 'axios'
export default {
  name: 'ProductsList',
  props: {
    forcedProducts: {
      type: Array,
      default () {
        return [];
      },
    }
  },
  data () {
    return {
      products: localStorage.products ? JSON.parse(localStorage.products) : [],
      timerId: null,
    }
  },
  watch: {
    forcedProducts (n) {
      if (n.length) {
        this.products = n;
        localStorage.products = JSON.stringify(this.products);
        this.clearIntervalForStock();
        this.setIntervalForStock();
      }
    }
  },
  async mounted () {
    if (!localStorage.products) {
      await axios.get('https://dummyjson.com/products?limit=100').then((response) => {
        if (response.data.products) {
          this.products = response.data.products;
          localStorage.products = JSON.stringify(this.products);
        }
      }).catch((e) => {
        console.error(e);
      });
    }
    this.setIntervalForStock();
  },
  beforeDestroy () {
    this.clearIntervalForStock();
  },
  computed: {
    stockIsExist () {
      return this.products.find(item => item.stock > 0);
    },
  },
  methods: {
    clearIntervalForStock () {
      clearInterval(this.timerId);
      this.timerId = null;
    },
    setIntervalForStock () {
      let time = 0;
      const ticks = this.products.map(() => this.getRandomSeconds());
      this.timerId = setInterval(() => {
        if (time === 4) {
          time = 1;
        } else {
          time += 1;
        }
        this.products.forEach((item, index) => {
            if (ticks[index] === time && item.stock !== 0) {
              item.stock -= 1;
            }
          });
        if (!this.stockIsExist) {
          clearInterval(this.timerId);
        }
      }, 1000);
    },
    getRandomSeconds () {
      return Math.floor(Math.random() * 4 + 1);
    },
    getBadge (order) {
      let badge;
      switch (order) {
        case 0:
          badge = {
            text: 'TOP',
            class: 'badge_left',
          }
          break;
        case 1:
          badge = {
            text: 'SALE',
            class: 'badge_center',
          }
          break;
        case 2:
          badge = {
            text: 'POPULAR',
            class: 'badge_bottom',
          }
          break;
        default:
          badge = null;
          break;
      }
      return badge;
    },
  }
}
</script>

<style lang="sass">
.products-list
  display: grid
  grid-template-columns: repeat(1, 1fr)
  gap: 8px 30px
.product-item
  overflow: hidden
  padding: 6px 7px 2px 7px
  border: 1px solid $gray-light
  &__badge
    position: absolute
    font-size: 10px
    line-height: 12px
    font-weight: 600
    color: $white
    padding: 4px 16px
    border-radius: 100px
  &__wrap-img
    position: relative
    margin-bottom: 2px
  &__img
    width: 100%
    height: 177px
  &__title
    font-weight: 600
  &__row
    margin-left: 2px
    &:not(:last-child)
      margin-bottom: 4px
  &__row_description
    display: flex
    align-items: flex-start
  &__multi-text
    -webkit-line-clamp: 1
    display: -webkit-box
    -webkit-box-orient: vertical
    overflow: hidden
  &__title_description
    margin-right: 3px
.badge
  &_left
    left: 8px
    top: 8px
    background-color: #71c396
  &_center
    left: 50%
    top: 8px
    transform: translateX(-50%)
    background-color: #e69349
  &_bottom
    right: 8px
    bottom: 8px
    background-color: #9b5de2
@media (min-width: 342px) and (max-width: 430px)
  .product-item
    padding: 7px 8px 5px 8px
    &__img
      height: 208px
    &__wrap-img
      margin-bottom: 4px
    &__row
      margin-left: 2px
      &:not(:last-child)
        margin-bottom: 8px
@media (min-width: 430px) and (max-width: 676px)
  .product-item
    padding: 9px 10px 10px 10px
    &__img
      height: 264px
    &__wrap-img
      margin-bottom: 8px
    &__row
      margin-left: 2px
      &:not(:last-child)
        margin-bottom: 8px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 14px
      line-height: 17px
@media (min-width: 677px) and (max-width: 834px)
  .products-list
    grid-template-columns: repeat(2, 1fr)
    gap: 16px 30px
  .product-item
    padding: 7px 7px 31px 7px
    &__img
      height: 201px
    &__wrap-img
      margin-bottom: 8px
    &__row
      margin-left: 2px
      &:not(:last-child)
        margin-bottom: 8px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 14px
      line-height: 17px
@media (min-width: 834px) and (max-width: 1024px)
  .products-list
    grid-template-columns: repeat(3, 1fr)
    gap: 16px 30px
  .product-item
    padding: 6px 6px 10px 6px
    &__img
      height: 161px
    &__wrap-img
      margin-bottom: 8px
    &__row
      font-size: 14px
      line-height: 17px
      margin-left: 2px
      &:not(:last-child)
        margin-bottom: 8px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 10px
      line-height: 12px
@media (min-width: 1024px) and (max-width: 1280px)
  .products-list
    grid-template-columns: repeat(3, 1fr)
    gap: 16px 30px
  .product-item
    padding: 7px 7px 23px 7px
    &__img
      height: 180px
    &__wrap-img
      margin-bottom: 8px
    &__row
      font-size: 14px
      line-height: 17px
      margin-left: 0px
      &:not(:last-child)
        margin-bottom: 8px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 12px
      line-height: 14.5px
@media (min-width: 1280px) and (max-width: 1440px)
  .products-list
    grid-template-columns: repeat(4, 1fr)
    gap: 16px 30px
  .product-item
    padding: 7px 7px 18px 7px
    &__img
      height: 176px
    &__wrap-img
      margin-bottom: 8px
    &__row
      font-size: 14px
      line-height: 17px
      margin-left: 0px
      &:not(:last-child)
        margin-bottom: 6px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 12px
      line-height: 14.5px
@media (min-width: 1440px) and (max-width: 1640px)
  .products-list
    grid-template-columns: repeat(4, 1fr)
    gap: 24px 30px
  .product-item
    padding: 8px 8px 32px 8px
    &__img
      height: 191px
    &__wrap-img
      margin-bottom: 8px
    &__row
      font-size: 14px
      line-height: 17px
      margin-left: 0px
      &:not(:last-child)
        margin-bottom: 8px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 12px
      line-height: 14.5px
@media (min-width: 1641px)
  .products-list
    grid-template-columns: repeat(4, 1fr)
    gap: 24px 30px
  .product-item
    padding: 9px 9px 34px 9px
    &__img
      height: 206px
    &__wrap-img
      margin-bottom: 9px
    &__row
      font-size: 16px
      line-height: 19px
      margin-left: 0px
      &:not(:last-child)
        margin-bottom: 8px
    &__multi-text
      -webkit-line-clamp: 2
    &__badge
      font-size: 14px
      line-height: 17px
</style>
