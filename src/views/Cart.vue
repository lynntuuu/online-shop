<template>
  <div class="wrapper">
    <h1>My Cart</h1>
    <ul>
      <li v-for="item in cartItems" :key="item.id">
        {{item.name}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'cart',
  computed: {
    cartItems () {
      return this.$store.getters.cartItems
    }
  }
}
</script>

<style lang="scss">
</style>
