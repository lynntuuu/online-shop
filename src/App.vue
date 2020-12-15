<template>
  <div id="nav" class="wrapper flex-col flex-col--align-center">
    <h1 class="flex-col--2">MyStore</h1>
    <div class="flex-col--2 nav-items">
      <router-link to="/" class="nav-items-item">Home</router-link>
      <router-link :to="{ name : 'gender-overview', params : { gender: 'women' }}"
      class="nav-items-item">Women</router-link>
      <router-link :to="{ name : 'gender-overview', params : { gender: 'women' }}"
      class="nav-items-item">Men</router-link>
      <router-link to="/cart" class="nav-items-item">Cart
        <counter-badge :count="cartCount"></counter-badge>
      </router-link>
    </div>
  </div>
  <router-view/>
</template>

<script>
import CounterBadge from '@/components/CounterBadge'

export default {
  name: 'app',
  components: { CounterBadge },
  computed: {
    cartCount () {
      // return the number of id’s in the “cart” by evaluating the array’s length
      return this.$store.state.cart.length
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
a {
  text-decoration: none;
  color: #2c3e50;
  transition: all .3s ease;
  &:hover {
    color: #42b983
  }
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.nav-items {
  justify-content: flex-end;
  display: flex;
}
.nav-items-item {
  margin-left: 1rem;
  position: relative;
}
ul {
  padding-left: 0;
  list-style: none;
}
</style>
