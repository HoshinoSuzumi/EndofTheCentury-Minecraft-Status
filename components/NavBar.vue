<template>
  <div class="np-navbar">
    <div class="np-navbar-title">End of The Century <b>Survival</b></div>
    <div class="np-navbar-actions">
      <!--      <button class="mdui-btn mdui-btn-icon mdui-text-color-grey-700">-->
      <!--        <i class="mdui-icon material-icons">settings</i>-->
      <!--      </button>-->
      <!--      <button class="mdui-btn mdui-text-color-grey-700">-->
      <!--        首页-->
      <!--      </button>-->
      <!--      <button class="mdui-btn mdui-text-color-grey-700">-->
      <!--        关于-->
      <!--      </button>-->
    </div>
  </div>
</template>

<script>
    export default {
        name: "NavBar",
    }
</script>

<style scoped>
  .np-navbar {
    width: 100%;
    height: 50px;
    display: flex;
    align-content: center;
    padding: 0 1.5rem;
    transition: all .3s;
    box-shadow: 0.5rem 0.875rem 2.375rem rgba(39, 44, 49, .06), 0.0625rem 0.1875rem 0.5rem rgba(39, 44, 49, .03);
    background: #FFFFFF;
  }

  .np-navbar.immerse {
    box-shadow: none;
    background: #FFFFFF00;
  }

  .np-navbar .np-navbar-title {
    width: 100%;
    margin: auto 0;
    font-size: 1.2rem;
    font-family: Poppins, "Microsoft JhengHei", sans-serif;
  }

  .np-navbar .np-navbar-actions {
    margin: auto 0;
    float: right;
    display: inline-flex;
    font-size: 1.2rem;
    font-family: "Microsoft JhengHei", sans-serif;
  }
</style>
