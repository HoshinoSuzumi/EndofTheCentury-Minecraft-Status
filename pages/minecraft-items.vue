<template>
  <div class="main">
    <ContentArea title="Minecraft 图鉴">
      <div>
        <label>
          <input class="np-search" spellcheck="false" autocomplete="off" type="text" placeholder="搜索方块或物品的名称、ID或标签"
                 v-model="inputKeyword"/>
        </label>
      </div>
    </ContentArea>
    <a-spin :spinning="loadingData" :delay="0" tip=" ">
      <a-icon slot="indicator" type="loading" style="font-size: 24px" spin/>
      <ContentArea>
        <div class="etcs-container">
          <div class="mdui-table-fluid">
            <table class="mdui-table mdui-table-hoverable">
              <thead>
              <tr>
                <th>ID</th>
                <th>图片</th>
                <th>名称</th>
                <th>标签</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="(Item, k) in MinecraftItems_search" :key="k">
                <td>{{ Item['id'] }}</td>
                <td><img :src="Item['icon']" :alt="Item['id']+':'+Item['name']"/></td>
                <td>{{ Item['name'] }}</td>
                <td>{{ Item['tag'] }}</td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </ContentArea>
    </a-spin>
  </div>
</template>

<script>
// TODO: 优化图鉴数据库读取机制，考虑分部分读取

/**
 * 关于网络请求
 * 
 * 直接将图鉴数据库存储在Vuex Store中（当前）
 * 优点：节省网络请求开销，且可以离线浏览
 * 缺点：每次进入页面会执行大量渲染操作导致明显卡顿
 * 
 * 网络请求图鉴数据库文件
 * 优点：可以在某种程度上防止客户端渲染大量数据时的卡顿
 * 缺点：无网络连接情况下无法浏览，且可能因为某些网络因素无法访问到图鉴数据库
 */

  import ContentArea from "../components/ContentArea";

  // let MC_ITEMS_LISTS = 'https://mc.boxmoe.cn/MinecraftFullyItems.json';

  export default {
    name: "minecraft-items",
    components: {ContentArea},
    data() {
      return {
        loadingData: true,
        inputKeyword: '',
        MinecraftItems: [],
        MinecraftItems_search: [],
      }
    },
    mounted() {
      // this.fetchItemsLists();
      this.MinecraftItems = this.$store.state.minecraftIds.MinecraftIDs;
      this.MinecraftItems_search = this.MinecraftItems;
      this.loadingData = false;
    },
    methods: {
      // fetchItemsLists() {
      //   this.$axios.get(MC_ITEMS_LISTS)
      //     .then((data) => {
      //       this.MinecraftItems = JSON.parse(JSON.stringify(data.data));
      //       console.log(this.MinecraftItems);
      //       this.MinecraftItems_search = this.MinecraftItems;
      //       this.loadingData = false;
      //     })
      //     .catch((e) => {
      //       this.loadingData = true;
      //       console.warn(e);
      //       let self = this;
      //       setTimeout(function () {
      //         self.fetchItemsLists();
      //       }, 2000);
      //     });
      // },
    },
    watch: {
      'inputKeyword': function (kw) {
        kw = kw.toLowerCase();
        let self = this;
        self.MinecraftItems_search = [];
        if (kw.toString().length === 0) {
          self.MinecraftItems_search = self.MinecraftItems;
          return null;
        }
        this.MinecraftItems.forEach(function (item) {
          if (item['id'].toLowerCase().indexOf(kw) !== -1) {
            self.MinecraftItems_search.push(item);
            return null;
          }
          if (item['name'].toLowerCase().indexOf(kw) !== -1) {
            self.MinecraftItems_search.push(item);
            return null;
          }
          if (item['tag'].toLowerCase().indexOf(kw) !== -1) {
            self.MinecraftItems_search.push(item);
            return null;
          }
        });
      }
    }
    ,
  }
</script>

<style scoped>
  .ant-spin-nested-loading {
    position: relative;
    width: 100%;
  }

  .mdui-table-fluid {
    border: none;
    box-shadow: none;
  }

  .mdui-table td, .mdui-table th {
    border-bottom: 1px solid rgba(0, 0, 0, .05);
  }
</style>
