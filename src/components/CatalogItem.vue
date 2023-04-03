<template>
  <div class="catalog-item-wrapper">
    <header>
      <h1 class="capitalize">{{productName}}</h1>
    </header>
    <section>
      <img :src="iconSrc" />
      <div class="grid">
        <strong>Type:</strong><p class="capitalize"><em>{{type}}</em></p>
        <strong>Price:</strong><p>{{currencyUnit}}{{Number.parseFloat(price).toFixed(2).toString()}}</p>
        <strong>Quantity:</strong>
        <input type="number" min="0" v-model="quantity"/> <!-- TODO: Add a label tag to the input later on -->
      </div>
    </section>
    <footer>
      <button @click="handleAddToCart">Add to Cart</button>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'CatalogItem',
  data()
  {
    return {
      quantity: '0'
    }
  },
  updated(){
    if(this.quantity == "") { this.quantity = 0; }
  },
  props: {
    id: {
      type: String,
      default: '0'
    },
    productName: {
      type: String,
      default: 'placeholder'
    },
    iconSrc: String,
    type: {
      type: String,
      default: 'placeholder'
    },
    currencyUnit: {
      type: String,
      default: '$'
    },
    price: {
      type: String,
      default: '0'
    },
  },
  methods: {
    handleAddToCart()
    {
      if(this.quantity > 0)
      {
        this.$emit('onAddToCart', {...this.$props, quantity: this.quantity.toString()});
      }
    }
  }
}
</script>

<style scoped>
  header{
    background-color: #CFD004;
    height: 40px;
  }

  header h1{
    font-size: 20px;
    color: #481F31;
    padding: 5px;
    font-weight: 500;
  }

  section{
    padding: 15px;
    background-color: #FCF0BB;
  }

  section img{
    height: 100px;
    margin-left: 50%;
    transform: translate(-50%);
    margin-bottom: 10px;
  }

  footer{
    background-color: #481F31;
    height: 40px;
    text-align: right;
  }

  footer button{
    color: #538636;
    background-color: #e4dfd9;
    font-weight: 900;
    padding: 5px;
    border: none;
    border-radius: 5px;
    margin-top: 5px;
    margin-right: 10px;
  }

  footer button:hover{
    cursor: pointer;
    background-color: #538636;
    color: #e4dfd9;
  }

  strong{
    margin-right: 10px;
    color: #481F31;
  }

  .catalog-item-wrapper{
    width: 100%;
  }

  .grid{
    display: grid;
    grid-template-columns: 1fr 1fr;
    row-gap: 10px;
  }

  .grid strong{
    text-align: right;
  }

  .grid input{
    width: 80px;
  }

  .capitalize{
    text-transform: capitalize;
  }
</style>