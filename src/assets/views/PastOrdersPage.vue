<template>
  <section>
    <div id="container">
      <h2>Past Orders</h2>
      <ProductList :productsToShow="pastOrders"/>
    </div>
  </section>
</template>

<script>
import ProductList from '../../components/ProductList.vue'
import pastOrdersJson from '../../data/pastOrders.json'

export default {
  name: 'ProductPage',
  data(){
    return {
      pastOrders: pastOrdersJson
    }
  },
  components: {
    ProductList
  },
}
</script>

<style scoped>
  #container{
    width: 70%;
    margin: 50px auto 300px auto;
  }

  #container h2{
    color: #481F31;
    margin: 10px;
  }
</style>