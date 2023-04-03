<template>
  <div id="wrapper">
    <aside>
      <header>
        <h2>Cart <img src="@/assets/img/navbar/cart.png" /></h2>
        <button @click="handleCloseCart">x</button>
      </header>
      <section>
        <p v-if="products == null || Object.keys(products).length == 0" class="no-items">No items in cart</p>
        <div v-else>
          <ProductList :productsToShow="products" :isRemovable="true" @onRemoveListItem="handleRemoveListItem"/>
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

export default {
  data(){
    return {
      total: 0
    }
  },
  props: {
    currencyUnit: {
      type: String,
      default: '$'
    },
    products: Object
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
    },
    updateTotal()
    {
      if(this.products != null)
      {
        let auxTotal = 0;

        for(let i=0; i < Object.keys(this.products).length; i++)
        {
          auxTotal += (Number.parseFloat(this.products[i].price) * Number.parseInt(this.products[i].quantity));
        }

        this.total = auxTotal.toFixed(2);
      }
    },
    handleRemoveListItem(index)
    {
      this.$emit('onRemoveListItem', index);
    }
  },
  updated()
  {
    this.updateTotal();
  }
}
</script>

<style scoped>
  #wrapper{
    position: fixed;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
  }

  aside{
    width: 400px;
    margin: 0 0 0 auto;
    color: white;
    height: 100%;
    pointer-events: auto;
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
    height: 95%;
    padding: 15px;
    background-color: #030014;
    font-size: 12px;
    overflow: auto;
  }

  section::-webkit-scrollbar{
    width: 10px;
    background-color: inherit;
  }

  section::-webkit-scrollbar-thumb{
    border: 1px solid white;
    background-color: inherit;
    border-radius: 20px;
  }

  .no-items{
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