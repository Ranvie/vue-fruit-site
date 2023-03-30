<template>
  <div id="wrapper">
    <aside>
      <header>
        <h2>Cart <img src="@/assets/img/navbar/cart.png" /></h2>
        <button @click="handleCloseCart">x</button>
      </header>
      <section>
        <p v-if="Object.keys(pastOrders).length == 0" class="no-items">No items in cart</p>
        <div v-else>
          <ProductList :productsToShow="pastOrders" :isRemovable="true"/>
          <div class="grid">
            <p class="total"><strong>Total:</strong> {{currencyUnit}}{{total}}</p>
            <button @click="handleCheckout">Checkout</button>
          </div>
        </div>
      </section>
    </aside>
  </div>
</template>

<script>
import ProductList from './ProductList.vue'
import pastOrdersJson from '../data/pastOrders.json'

export default {
  data(){
    return {
      pastOrders: pastOrdersJson,
      total: 0
    }
  },
  props: {
    currencyUnit: {
      type: String,
      default: '$'
    }
  },
  components: {
    ProductList
  },
  methods: {
    handleCloseCart()
    {
      this.$emit('onCloseCart', 'cart');
    },
    handleCheckout()
    {
      this.$emit('onCheckout');
    }
  },
  created()
  {
    for(let i=0; i < Object.keys(this.pastOrders).length; i++)
    {
      this.total += (Number.parseFloat(this.pastOrders[i].price) * Number.parseInt(this.pastOrders[i].quantity));
    }

    this.total = this.total.toFixed(2);
  }
}
</script>

<style scoped>
  #wrapper{
    position: fixed;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 1; /* Dar preferência de seleção para a página, não para o carrinho */
  }

  aside{
    width: 400px;
    margin: 0 0 0 auto;
    color: white;
    height: 100%;
  }

  header{
    background-color: #2F2B4F;
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 5px 10px;
  }

  header img{
    height: 30px;
    transform: translateY(20%);
  }

  header button{
    width: 30px;
    border: none;
    background-color: #2F2B4F;
    margin: 0 0 0 auto;
    font-weight: 900;
    font-size: 25px;
    color: white;
  }

  header button:hover{
    cursor: pointer;
  }

  section{
    height: 100%;
    padding: 15px;
    background-color: #030014;
    font-size: 12px;
  }

  section p.noItems{
    font-style: italic;
    font-size: 15px;
    text-align: center;
  }

  .grid{
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 10px;
  }

  .grid p.total{
    margin-top: 15px;
    font-size: 15px;
  }

  .grid button{
    height: 30px;
    width: 100px;
    color: #538636;
    background-color: #e4dfd9;
    font-weight: 900;
    border: none;
    border-radius: 5px;
    transform: translateY(30%);
    margin: 0 0 0 auto;
  }

  .grid button:hover{
    cursor: pointer;
    background-color: #538636;
    color: #e4dfd9;
  }

</style>