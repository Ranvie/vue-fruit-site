<template>
  <section>
    <div id="container">
      <h2>Products</h2>
      <section id="catalog-items">
        <CatalogItem 
          v-for="(product, i) in products" :key="i"
          :id="product.id" 
          :productName="product.productName"
          :type="product.type" 
          :price="product.price" 
          :iconSrc="Utils.requireImg(product.iconSrc)" 
          @onAddToCart="handleAddToCart"
        />
      </section>
    </div>
  </section>
</template>

<script>
import CatalogItem from '../../components/CatalogItem.vue';
import productJSON from '../../data/products.json';
import UtilMethods from '../../shared/UtilMethods.vue';

export default {
  name: 'ProductPage',
  data(){
    return {
      products: productJSON,
      Utils: UtilMethods
    }
  },
  components: {
    CatalogItem,
  },
  methods: {
    handleAddToCart(cartItem)
    {
      this.$emit('onAddToCart', cartItem);
    }
  }
}
</script>

<style scoped>
  #container{
    width: 80%;
    margin: 50px auto 300px auto;
  }

  #container h2{
    color: #481F31;
    margin: 10px;
  }

  #catalog-items{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    row-gap: 20px;
  }

  @media(max-width: 768px){
    #catalog-items{
      grid-template-columns: 1fr 1fr;
    }
  }

  @media(max-width: 576px){
    #catalog-items{
      grid-template-columns: 1fr;
    }
  }
</style>