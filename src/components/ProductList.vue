<template>
  <table v-if="this.productsToShow != null && Object.keys(this.productsToShow).length > 0">
    <tr>
      <th></th>
      <th>Product</th>
      <th>Price</th>
      <th>Quantity</th>
      <th>Total</th>
      <th v-if="isRemovable"></th>
    </tr>
    <ProductListItem
      v-for="(product, i) in productsToShow"
      :key="i"
      :iconSrc="product.iconSrc"
      :productName="product.productName"
      :currencyUnit="product.currencyUnit"
      :price="product.price"
      :quantity="product.quantity"
      :isRemovable="isRemovable"
      :index="Number.parseInt(i)"
      @onRemoveListItem="removeListItem"
    />
  </table>
</template>

<script>
import ProductListItem from './ProductListItem.vue'
import UtilMethods from '../shared/UtilMethods.vue';

export default {
  name: 'ProductList',
  data(){
    return {
      Util: UtilMethods
    }
  },
  methods: {
    removeListItem(index)
    {
      this.$emit('onRemoveListItem', index);
    }
  },
  props: {
    productsToShow: {
      iconSrc: 'null',
      productName: 'empty',
      currencyUnit: '$',
      price: '0',
      quantity: '0'
    },
    isRemovable: {
      type: Boolean,
      default: false
    }
  },
  components: {
    ProductListItem
  }
}
</script>

<style scoped>
  table th{
    padding: 10px 18px;
  }
</style>