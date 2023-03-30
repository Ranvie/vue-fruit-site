<template>
  <div>
    <header>
      <NavBar @navbarClicked="handleNavbarClick" @onToggleCart="toggleCart"/>
    </header>
    <main>
      <HomePage v-if="isHomePageVisible"/>
      <ProductPage v-if="isProductPageVisible" />
      <PastOrdersPage v-if="isPastOrdersPageVisible" />
      <SideCart v-show="isCartVisible" @onCloseCart="toggleCart"/>
    </main>
    <footer>

    </footer>
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import HomePage from './assets/views/HomePage.vue'
import ProductPage from './assets/views/ProductPage.vue'
import PastOrdersPage from './assets/views/PastOrdersPage.vue'
import SideCart from './components/SideCart.vue'

export default {
  name: 'App',
  data() {
    return {
      isHomePageVisible: true,
      isProductPageVisible: false,
      isPastOrdersPageVisible: false,
      isCartVisible: true
    }
  },
  methods: {
    handleNavbarClick(clicked)
    {
      this.togglePages(clicked);
    },

    togglePages(clicked)
    {
      switch(clicked)
      {
        case 'home':
          this.activePage(true, false, false);
          break;

        case 'products':
          this.activePage(false, true, false);
          break;

        case 'pastOrders':
          this.activePage(false, false, true);
          break;
      }
    },
    activePage(home, products, pastOrders)
    {
      this.isHomePageVisible = home;
      this.isProductPageVisible = products;
      this.isPastOrdersPageVisible = pastOrders;
    },
    toggleCart()
    {
      this.isCartVisible = !this.isCartVisible;
    }
  },
  components: {
    NavBar,
    HomePage,
    ProductPage,
    PastOrdersPage,
    SideCart
  }
}
</script>

<style>
  * {
    margin: 0;
    box-sizing: border-box;
  }

  body{
    background-color: #FCF3D1;
  }

  footer{
    height: 50px;
    background-color: #481F31;
  }

  #app{
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
</style>
