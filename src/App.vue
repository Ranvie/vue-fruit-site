<template>
  <div>
    <header>
      <NavBar @navbarClicked="handleNavbarClick" @onToggleCart="toggleCart" :itemsOnCart="Object.keys(this.cart).length.toString()"/>
    </header>
    <main> <!-- TODO: Talvez trocar pelo VueRouter seja melhor, estudar sobre -->
      <HomePage v-if="isHomePageVisible" @onAddToCart="addToCart" />
      <ProductPage v-if="isProductPageVisible" @onAddToCart="addToCart" />
      <PastOrdersPage v-if="isPastOrdersPageVisible" :receivedPastOrders="pastOrders"/>
      <SideCart v-show="isCartVisible" @onCloseCart="toggleCart" :products="cart" @onRemoveListItem="removeListItem" @onCheckout="handleCheckout"/>
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
      isCartVisible: false,
      cart: [],
      pastOrders: {}
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
    },
    addToCart(addedItem)
    {
      this.cart.push(addedItem);
    },
    removeListItem(index)
    {
      let aux = [];

      for(let i=0; i < this.cart.length; i++)
      {
        if(i != index)
        {
          aux.push(this.cart[i]);
        }
      }

      this.cart = aux;
    },
    handleCheckout()
    {
      let totalOrders = this.pastOrders != {} ? this.cart.concat(...Object.values(this.pastOrders)) : this.cart;
      this.pastOrders = this.arrayToObj(totalOrders); //TODO: Criar um único objeto que tenha todos os outros, da forma que está, a cada novos itens adicionados, está separando o antigo do novo;
      console.log(this.pastOrders);

      //let cartJson = JSON.stringify(cartConvToObj);
      //TODO: O Browser não permite salvar em arquivos;
      this.cart = [];
    },
    arrayToObj(array = [])
    {
      let aux = {};
      let keyID = 0;

      for(let element of array)
      {
        if(Object.keys(element).length > 0)
        {
          aux[keyID] = element;
          keyID++;
        }
      }

      return aux;
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
