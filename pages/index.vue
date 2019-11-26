<template>
  <div>
    <div class="title">
      <h1>End of The Century</h1>
      <p>“末世纪” 纯净综合服务器</p>
      <div class="mdui-container etcs-container container" style="background: rgba(255,255,255,.5);">
        <div class="mdui-table-fluid mdui-typo">
          <table class="mdui-table">
            <thead>
            <tr>
              <th>IP</th>
              <th>版本</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td>etcs.redneno.me</td>
              <td>1.14.4</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "index"
  }
</script>

<style scoped>
  .title {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 80%;
    height: calc(100vh - 50px);
    margin: 0 auto;
  }

  .title h1 {
    color: #FFFFFF;
    margin: 0 auto;
    font-size: 2.2rem;
    font-weight: 100;
  }

  .title p {
    color: #FFFFFF;
  }

  .container {
    width: unset;
    padding: 1rem;
    margin-top: 2rem;
    background: rgba(255, 255, 255, .5) !important;
    backdrop-filter: blur(20px);
  }


  .mdui-table-fluid {
    border: none;
    box-shadow: none;
  }

  .mdui-table {
    background: rgba(0, 0, 0, 0);
  }

  .mdui-table td, .mdui-table th {
    border-bottom: 1px solid rgba(0, 0, 0, .05);
  }
</style>
