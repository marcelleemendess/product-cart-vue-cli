<template>
   <header class="top-bar spread">
        <nav class="top-bar-nav">
          <router-link to="/" class="top-bar-link">
            <i class="icofont-spoon-and-fork"></i>
            <span>Home</span>
          </router-link>
          <router-link to="/products" class="top-bar-link">
            <span>Products</span>
          </router-link>
          <router-link to="views/past-orders" class="top-bar-link">
            <span>Past Orders</span>
          </router-link>
      </nav>
      <div @click="toggleSidebar" class="top-bar-cart-link">
        <i class="icofont-cart-alt icofont-1x"></i>
        <span>Cart ({{ totalQuantity }})</span>
      </div>
    </header>
  <router-view :inventory="inventory" :addToCart="addToCart"/>

  <SidebarMenu
     v-if="showSidebar"
      v-bind:toggle="toggleSidebar"
      :cart="cart"
      :inventory="inventory"
      :remove="removeItem"
  />

</template>

<script>
import SidebarMenu from '@/components/SidebarMenu.vue'
import food from './food.json'

export default {
  components: {
    SidebarMenu
  },
  data () {
    return {
      showSidebar: false,
      inventory: food,
      cart: {}
    }
  },
  computed: {
    totalQuantity () {
      return Object.values(this.cart).reduce((sum, curr) => {
        return sum + curr
      }, 0) // 0 is the initial value
    }
  },
  methods: {
    addToCart (name, quantity) {
      if (!this.cart[name]) this.cart[name] = 0
      this.cart[name] += quantity
    },
    toggleSidebar () {
      this.showSidebar = !this.showSidebar
    },
    removeItem (name) {
      delete this.cart[name]
    }
  }
}
</script>
