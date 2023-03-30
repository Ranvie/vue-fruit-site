<template>
  <tr>
    <td><img :src="iconSrc"></td>
    <td>{{productName}}</td>
    <td>{{currencyUnit}}{{Number.parseFloat(price).toFixed(2).toString()}}</td>
    <td>{{quantity}}</td>
    <td>{{currencyUnit}}{{total}}</td>
    <td v-if="isRemovable"><button @click="handleRemoveItem(this.index)">x</button></td>
  </tr>
</template>

<script>
export default {
  data(){
    return {
      total: 0
    }
  },
  props: {
    iconSrc: String,
    productName: String,
    currencyUnit: {
      type: String,
      default: '$'
    },
    price: {
      type: String,
      default: '0'
    },
    quantity: String,
    isRemovable: {
      type: Boolean,
      default: false
    },
    index: Number
  },
  methods: {
    handleRemoveItem(index)
    {
      this.$emit('onRemoveListItem', index);
    }
  },
  mounted() {
    this.total = (Number.parseFloat(this.price) * Number.parseInt(this.quantity)).toFixed(2);
  }
}
</script>

<style scoped>
  td{
    text-align: center;
  }

  img{
    height: 30px;
  }

  button{
    color: #538636;
    background-color: #e4dfd9;
    font-weight: 900;
    padding: 4px 8px;
    border: none;
    border-radius: 5px;
  }

  button:hover{
    cursor: pointer;
    background-color: #538636;
    color: #e4dfd9;
  }
</style>