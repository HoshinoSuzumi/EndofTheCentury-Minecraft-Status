<template>
  <div class="main">
    <ContentArea title="Minecraft IDs">
      <div>
        <label>
          <input class="np-search" spellcheck="false" autocomplete="off" type="text" placeholder="搜索方块或物品的名称、ID或标签"
                 v-model="inputKeyword"
                 autofocus/>
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
              <tr v-for="(Item, k) in MinecraftItems" :key="k">
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
    import ContentArea from "../components/ContentArea";

    // const MC_ITEMS_LISTS = 'https://bmss2-1253315888.file.myqcloud.com/static/other/Minecraft/MinecraftFullyItems.json';
    let MC_ITEMS_LISTS = '/MinecraftFullyItems.json';

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
            this.fetchItemsLists();
        },
        methods: {
            fetchItemsLists() {
                this.$axios.get(MC_ITEMS_LISTS)
                    .then((data) => {
                        this.MinecraftItems = JSON.parse(JSON.stringify(data.data));
                        this.loadingData = false;
                    })
                    .catch((e) => {
                        this.loadingData = true;
                        console.warn(e);
                        let self = this;
                        setTimeout(function () {
                            self.fetchItemsLists();
                        }, 2000);
                    });
            },
            searchItems(keyword) {
                return keyword;
            },
        },
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
