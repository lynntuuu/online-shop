<template>
  <div class="wrapper">
    <h1>My Cart</h1>
    <ul class="cart-list">
      <li class="flex-col cart-list-item" v-for="item in cartItems" :key="item.id">
        <img :src="makeImagePath(item)" class="pic" alt="">
        <div class="flex-col cart-list-item-detail">
          <div>
            <p>{{ item.name }}</p>
            <p>Size: {{ item.size }}</p>
            <p>Color: {{ item.color }}</p>
          </div>
        </div>
        <p>${{ item.price }}</p>
        <button
          @click="removeFromCart(item.id)"
          class="btn cart-list__btn-remove">
          Remove
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
import { imagePath } from '@/mixins/imagePath.js'

export default {
  name: 'cart',
  mixins: [imagePath],
  computed: {
    cartItems () {
      return this.$store.getters.cartItems
    }
  },
  methods: {
    removeFromCart (itemId) {
      this.$store.dispatch('removeFromCart', itemId)
    }
  }
}
</script>

<style lang="scss">
.cart-list {
  width: 70%;
  margin-right: 1rem;
  @media only screen and (max-width: 832px) {
    width: 100%
  }
}
.cart-list-item {
  width: 100%;
  border-bottom: 2px solid #333333;
}
.pic {
  max-width: 50px;
  margin-top: .5rem;
}
.cart-list-item-detail {
  flex: 2;
  justify-content: space-between;
  margin-left: 2rem;
}
.cart-list-btn-remove {
  margin-top: .5rem;
  &:hover {
    color: red;
  }
}
</style>
