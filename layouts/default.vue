<template>
  <div>
    <NavBar/>
    <div class="etcs-background-mask">
      <nuxt/>
    </div>
  </div>
</template>

<script>
  import NavBar from "../components/NavBar";

  export default {
    components: {NavBar}
  }
</script>

<style>
  /**
   * 关于 backdrop-filter
   * 这个属性似乎没有得到良好的处理，它在我的 Chrome 中表现的不尽人意，并且会大幅降低流畅度
   */
  body {
    padding-top: 50px;
  }

  html, body {
    background-image: url("https://picbed-1253315888.file.myqcloud.com/images/etcphoto.png ");
    background-repeat: no-repeat;
    background-position: center;
    background-attachment: fixed;
    background-size: cover;
  }

  .etcs-background-mask {
    min-height: calc(100vh - 50px);
    /* backdrop-filter: blur(10px); */
    background-color: rgba(255, 255, 255, .35);
  }
</style>
