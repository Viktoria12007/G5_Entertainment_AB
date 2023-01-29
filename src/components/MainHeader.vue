<template lang="pug">
header.header
  div.container
    a.logo__link.hide(href="#")
      img.logo__img(src="../assets/logo.png", alt="Логотип")
    button Update Products
</template>

<script>
export default {
  name: 'MainHeader'
}
</script>

<style lang="sass">
.header
  display: flex
  justify-content: space-between
  height: 48px
  background-color: $primary
</style>
