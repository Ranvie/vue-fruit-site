<script>
export default {
  requireImg(imgSrc)
  {
    try
    {
      return require(`@/${imgSrc}`);
    }
    catch(err)
    {
      console.error(`${err.name}: Cannot find image.`);
      return require(`@/assets/img/error/notFound.png`);
    }
  }
}
</script>