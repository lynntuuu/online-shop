<template>
  <div class="wrapper">
    <h1>My Cart</h1>
    <div class="flex-col">
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
      <section class="total-section">
        <ul class="total-section-list">
          <li class="total-section-item">
            <p class="total-section-item-label">{{ cartItemsCount }} items</p>
            <p>{{ itemsSubtotal }}</p>
          </li>
          <li class="total-section-item">
            <p class="total-section-item-label">Shipping</p>
            <select v-model="selectedShippingOption">
              <option disabled value="">Please select an option</option>
              <option v-for="option in shippingOptionsArray"
                :key= "option.text"
                :value= "option.rate">
              {{ option.text }} (${{ option.rate }})
              </option>
            </select>
          </li>
          <li class="total-section-item">
            <p class="total-section-item-label">Subtotal</p>
            <p>{{ subtotal }}</p>
          </li>
          <li class="total-section-item">
            <p class="total-section-item-label">Tax ({{ salesTaxPercentage }})</p>
            <p>{{ salesTaxApplied }}</p>
          </li>
          <li class="total-section-item">
            <p class="total-section-item-label">Total</p>
            <p>{{ total }}</p>
          </li>
        </ul>
        <button class="btn btn-grey total-section-checkout-button"
        :disabled="!this.selectedShippingOption">Check out</button>
      </section>
    </div>
  </div>
</template>

<script>
import { imagePath } from '@/mixins/imagePath.js'

export default {
  name: 'cart',
  mixins: [imagePath],
  data () {
    return {
      salesTax: 0.06,
      selectedShippingOption: '',
      shippingOptionsArray: [
        {
          text: 'One day',
          rate: 20
        },
        {
          text: 'Two days',
          rate: 15
        },
        {
          text: 'Three to five days',
          rate: 10
        },
        {
          text: 'One week or more',
          rate: 5
        }
      ]
    }
  },
  computed: {
    cartItems () {
      return this.$store.getters.cartItems
    },
    cartItemsCount () {
      return this.cartItems.length
    },
    itemsSubtotal () {
      return this.cartItems.reduce((total, item) => total + item.price, 0)
    },
    subtotal () {
      if (this.selectedShippingOption) {
        return Number(this.itemsSubtotal) + Number(this.selectedShippingOption)
      } else {
        return '---'
      }
    },
    salesTaxPercentage () {
      return `${this.salesTax * 100}%`
    },
    salesTaxApplied () {
      // subtotal * salesTax
      if (this.selectedShippingOption) {
        return (this.subtotal * this.salesTax).toFixed(2)
      } else {
        return '---'
      }
    },
    total () {
      // subtotal + salesTaxApplied
      if (this.selectedShippingOption) {
        return Number(this.subtotal) + Number(this.salesTaxApplied)
      } else {
        return '---'
      }
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
.total-section {
  background-color: #FAFAFA;
  padding: 0 1rem 1rem;
}
.total-section-list {
  margin: 0;
}
.total-section-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.total-section-item-label {
  font-weight: bold;
  margin-right: 1rem
}
.total-section-checkout-button {
  width: 100%;
}
</style>
