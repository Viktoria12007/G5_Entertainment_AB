<template lang="pug">
  section.products-list__section
    div.container
      div.products-list
        div.product-item.products-list__item(
          v-for="(product, index) in products",
          :key="product.id")
          div.product-item__wrap-img
            img.product-item__img(:src="product.thumbnail", :alt="product.title")
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
            div.product-item__title.product-item__title_description {{ 'Description: ' }}
            div.product-item__text.product-item__multi-text {{ ' ' + product.description }}
              //span.product-item__text.product-item__multi-text {{ product.description }}
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
      timersId: [],
    }
  },
  watch: {
    forcedProducts (n) {
      if (n.length) {
        this.products = n;
        localStorage.products = JSON.stringify(this.products);
        this.clearAllIntervalForStock();
        this.setIntervalForStock();
      }
    }
  },
  async mounted () {
    if (!localStorage.products) {
      const response = await axios.get('https://dummyjson.com/products?limit=100');
      this.products = response.data.products;
      localStorage.products = JSON.stringify(this.products);
    }
    this.setIntervalForStock();
  },
  beforeDestroy () {
    this.clearAllIntervalForStock();
  },
  methods: {
    clearAllIntervalForStock () {
      this.timersId.forEach((timer) => {
        clearInterval(timer);
      });
      this.timersId = [];
    },
    setIntervalForStock () {
      this.products.forEach((item) => {
        const timer = setInterval(() => {
          item.stock -= 1
          if (item.stock === 0) {
            clearInterval(timer);
          }
        }, 1000 * this.getRandomSeconds());
        this.timersId.push(timer);
      });
    },
    getRandomSeconds () {
      return Math.floor(Math.random() * (4 - 1 + 1) + 1);
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
  display: flex
  flex-wrap: wrap
  align-items: center
  &__item
    &:not(:last-child)
      margin-bottom: 8px
.product-item
  overflow: hidden
  padding: 6px 6px 3px 6px
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
  &__row_description
    display: flex
    align-items: center
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
</style>
