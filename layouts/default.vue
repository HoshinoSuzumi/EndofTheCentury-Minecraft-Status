<template>
  <div>
    <NavBar/>
    <nuxt/>
  </div>
</template>

<script>
  import NavBar from "../components/NavBar";

  export default {
    components: {NavBar}
  }
</script>

<style>
  body {
    padding-top: 50px;
  }
</style>
