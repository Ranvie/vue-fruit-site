<template>
  <section>
    <img :src="bannerScr">
    <h1>{{bannerTitle}}</h1>
  </section>
</template>

<script>
export default {
  name: 'BigBanner',
  props: {
    bannerScr: String,
    bannerTitle: String
  }
}
</script>

<style scoped>
  img{
    height: 500px;
    width: 100%;
    z-index: 0;
  }

  h1{
    position: absolute;
    top: 50%;
    width: 100%;
    height: 50%;
    text-align: center;
    transform: translateY(-50%);
    color: #ED5151;
    font-size: 130px;
    z-index: 1;
    font-family: 'Righteous', cursive;
    font-weight: 1000;
    -webkit-text-stroke-width: 0.5px;
    -webkit-text-stroke-color: #481F31;
  }
</style>