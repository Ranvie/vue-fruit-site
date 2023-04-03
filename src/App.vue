<!-- TODO: Colocar os itens da lista do carrinho em cookie, de forma que carreguem quando abrir a página 
           A página de ordens passadas fica com o rodapé no lugar errado para telas pequenas
-->

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
    <footer id="footer" class="sticky">

    </footer>
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import HomePage from './assets/views/HomePage.vue'
import ProductPage from './assets/views/ProductPage.vue'
import PastOrdersPage from './assets/views/PastOrdersPage.vue'
import SideCart from './components/SideCart.vue'
import VueCookie from 'vue-cookie'
import ProductJson from './data/products.json'
import UtilMethods from './shared/UtilMethods.vue'

let footer;

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
  mounted()
  {
    footer = document.getElementById("footer");

    document.body.addEventListener('DOMSubtreeModified', () => {
      this.toggleStickyFooter();
    });
  },
  created()
  {
    if(navigator.cookieEnabled)
    {
      let content = VueCookie.get("cart-items");

      if(content != null)
      {
        this.loadCookieCart(JSON.parse(content));
      }
    }
  },
  methods: {
    loadCookieCart(items = [])
    {
      for(let item of items)
      {
        if(!this.isNumber(item.id)) { continue; }
        this.cart.push({...ProductJson[item.id], quantity: item.qty});
      }

      for(let item of this.cart)
      {
        if(!item.iconSrc.includes('data:'))
        {
          item.iconSrc = UtilMethods.requireImg(item.iconSrc);
        }
      }
    },
    isNumber(value)
    {
      return !isNaN(Number.parseInt(value));
    },
    setupCookieCart(cart = [])
    {
      let cookieCart = [];

      for(let i=0; i < cart.length; i++)
      {
        cookieCart.push({id: `${cart[i].id}`, qty: `${cart[i].quantity}`});
      }

      return cookieCart;
    },
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
    toggleStickyFooter()
    {
      let bodyHeight = document.body.offsetHeight;
      let viewportHeight = window.innerHeight;

      if(bodyHeight > viewportHeight)
      {
        footer.classList.remove("sticky");
      }
      else
      {
        footer.classList.add("sticky");
      }
    },
    toggleCart()
    {
      this.isCartVisible = !this.isCartVisible;
    },
    addToCart(addedItem)
    {
      this.cart.push(addedItem);
      if(navigator.cookieEnabled) { VueCookie.set("cart-items", JSON.stringify(this.setupCookieCart(this.cart)), { expires: '1M' }); }
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
      if(navigator.cookieEnabled) { VueCookie.set("cart-items", JSON.stringify(this.setupCookieCart(this.cart)), { expires: '1M' }); }
    },
    handleCheckout()
    {
      let totalOrders = this.pastOrders != {} ? this.cart.concat(...Object.values(this.pastOrders)) : this.cart;
      this.pastOrders = this.arrayToObj(totalOrders);

      this.cart = [];
      VueCookie.set("cart-items", JSON.stringify([]));
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

  footer.sticky{
    position: absolute;
    width: 100%;
    top: 100%;
    transform: translateY(-100%);
  }

  #app{
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
</style>
