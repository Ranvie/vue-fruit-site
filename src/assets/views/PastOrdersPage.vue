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
import UtilMethods from '../../shared/UtilMethods.vue'

export default {
  name: 'ProductPage',
  data(){
    return {
      pastOrders: this.receivedPastOrders
    }
  },
  props: {
    receivedPastOrders: Object
  },
  methods: {
    requireIcons(){
      if(this.pastOrders != null)
      {
        for(let i=0; i < Object.keys(this.pastOrders).length; i++)
        {
          if(!this.pastOrders[i].iconSrc.includes('data:'))
          {
            this.pastOrders[i].iconSrc = UtilMethods.requireProdIcon(this.pastOrders[i]);
          }
        }
      }
    }
  },
  mounted(){
    this.requireIcons();
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