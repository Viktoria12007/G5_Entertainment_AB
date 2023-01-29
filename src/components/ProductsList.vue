<template lang="pug">
  section.products-list__section
    div.container
      div.products-list
        div.products-item.products-list__item(
          v-for="(product, index) in products",
          :key="product.id")
          div.products-item__wrap-img
            img.product-item__img(:src="product.thumbnail", :alt="product.title")
            div.products-item__badge(
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
  data () {
    return {
      products: localStorage.products ? JSON.parse(localStorage.products) : [],
    }
  },
  async mounted () {
    if (!localStorage.products) {
      const response = await axios.get('https://dummyjson.com/products?limit=100');
      this.products = response.data.products;
      localStorage.products = JSON.stringify(this.products);
    }
    this.products.forEach((item) => {
      const timer = setInterval(() => {
        item.stock -= 1
        if (item.stock === 0) {
          clearInterval(timer);
        }
      }, 1000 * this.getRandomSeconds());
    });
  },
  methods: {
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
.badge_left
  left: 8px
  top: 8px
  background-color: #71c396
.badge_center
  left: 50%
  top: 8px
  transform: translateX(-50%)
  background-color: #e69349
.badge_bottom
  right: 8px
  bottom: 8px
  background-color: #9b5de2
.products-item__badge
  position: absolute
  font-size: 10px
  line-height: 12px
  font-weight: 600
  color: $white
  padding: 4px 16px
  border-radius: 100px
.products-item__wrap-img
  position: relative
  margin-bottom: 2px
.product-item__img
  width: 100%
  height: 177px
.product-item__row_description
  display: flex
  align-items: center
.product-item__title_description
  margin-right: 3px
.products-list
  display: flex
  flex-wrap: wrap
  align-items: center
.products-item
  overflow: hidden
  padding: 6px 6px 3px 6px
  border: 1px solid $gray-light
.products-list__item
  &:not(:last-child)
    margin-bottom: 8px
.product-item__row
  //display: inline-block
  //white-space: nowrap
.product-item__title
  font-weight: 600
.product-item__multi-text
  -webkit-line-clamp: 1
  display: -webkit-box
  -webkit-box-orient: vertical
  overflow: hidden
</style>
