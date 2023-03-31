<template>
  <section>
    <BigBanner :bannerScr="require('../img/banner/food-banner.jpg')" bannerTitle="Splendid Food"/>
    <div id="container">
      <h2>Recomended</h2>
      <section id="catalog-items">
        <CatalogItem v-for="(product, i) in recommended" :key="i" 
          :productName="product.productName" 
          :type="product.type" 
          :price="product.price" 
          :iconSrc="Util.requireProdIcon(product)" 
          @onAddToCart="handleAddToCart"
        />
      </section>
    </div>
  </section>
</template>

<script>
import BigBanner from '../../components/BigBanner.vue'
import CatalogItem from '../../components/CatalogItem.vue';
import productJSON from '../../data/products.json';
import UtilMethods from '../../shared/UtilMethods.vue';

export default {
  name: 'HomePage',
  data(){
    return {
      products: productJSON,
      recommended: [],
      Util: UtilMethods
    }
  },
  components: {
    CatalogItem,
    BigBanner
  },
  created(){
    this.recommended = this.generateRecommended(3); //TODO: Randomizar apenas quando a página carrega, não quando o elemento é criado
  },
  methods: {
    handleAddToCart(cartItem)
    {
      this.$emit('onAddToCart', cartItem);
    },
    generateRecommended(quantity)
    {
      let productIndex = [];
      let recommProds = [];

      do{
        let nextInt = this.randInteger(0, this.getObjLength(this.products)-1);

        if(!productIndex.includes(nextInt))
        {
          productIndex.push(nextInt);
        }
      }while(productIndex.length < quantity);

      for(let i=0; i < quantity; i++)
      {
        recommProds.push(this.products[productIndex[i]]);
      }

      return recommProds;
    },
    randInteger(minIncluded, maxIncluded) {
      return Math.floor(Math.random() * (maxIncluded - minIncluded + 1) ) + minIncluded;
    },
    getObjLength(obj)
    {
      return Object.keys(obj).length;
    }
  }
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

  #catalog-items{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    row-gap: 20px;
  }

  #catalog-items CatalogItem{
    margin: 20px;
  }
</style>