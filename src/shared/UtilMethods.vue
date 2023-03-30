<script>
export default {
  requireProdIcon(product)
  {
    try
    {
      return require(`@/${product.iconSrc}`);
    }
    catch(err)
    {
      console.error(`${err.name}: Cannot find image.`);
      return require(`@/assets/img/error/notFound.png`);
    }
  }
}
</script>