<template>
  <nav>
    <ul>
      <a @click="handleNavbarClick('home')"><li><img src="../assets/img/navbar/homeIcon.png">Home</li></a>
      <a @click="handleNavbarClick('products')"><li>Products</li></a>
      <a @click="handleNavbarClick('pastOrders')"><li>Past Orders</li></a>
    </ul>
    <div class="cart-container">
      <a @click="handleToggleCart"><img src="../assets/img/navbar/cart.png">Cart({{itemsOnCart}})</a>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  props: {
    itemsOnCart: {
      type: String,
      default: '0'
    }
  },
  methods: {
    handleNavbarClick(clicked)
    {
      this.$emit('navbarClicked', clicked);
    },
    handleToggleCart()
    {
      this.$emit('onToggleCart');
    }
  }
}
</script>

<style scoped>
  nav{
    background-color: #7daa01;
    display: grid;
    grid-template-columns: 1fr 1fr;
  }

  nav > div{
    padding: 10px;
    margin-right: 20px;
    width: auto;
    text-align: right;
  }

  nav img{
    height: 15px;
    margin-right: 15px;
    transform: scale(200%)
  }

  ul{
    padding: 0;
    margin: 0;
    list-style-type: none;
  }

  li{
    display: inline-block;
    padding: 10px 20px;
    border-right: 1px solid white;
  }

  li:hover{
    cursor: pointer;
    background-color: rgba(255, 255, 255, 0.2);
    color: #486300;
  }

  a{
    text-decoration: none;
    color: white;
  }

  button{
    width: 90px;
    height: 50px;
  }

  .cart-container a:hover{
    cursor: pointer;
    color: #486300;
  }
</style>