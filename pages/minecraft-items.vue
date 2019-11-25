<template>
  <div class="main">
    <ContentArea title="Minecraft IDs">
      <div>
        <label>
          <input class="np-search" spellcheck="false" type="text" placeholder="搜索方块或物品的名称、ID或标签" autofocus/>
        </label>
      </div>
    </ContentArea>
    <a-spin :spinning="loadingData" :delay="0" tip=" ">
      <a-icon slot="indicator" type="loading" style="font-size: 24px" spin/>
      <ContentArea>
        <div class="etcs-container">
          {{ MinecraftItems }}
        </div>
      </ContentArea>
    </a-spin>
  </div>
</template>

<script>
    import ContentArea from "../components/ContentArea";

    const MC_ITEMS_LISTS = 'https://bmss2-1253315888.file.myqcloud.com/static/other/Minecraft/MinecraftFullyItems.json';

    export default {
        name: "minecraft-items",
        components: {ContentArea},
        data() {
            return {
                loadingData: true,
                MinecraftItems: [],
            }
        },
        mounted() {
            // this.fetchItemsLists();
        },
        methods: {
            fetchItemsLists() {
                this.$axios.get(MC_ITEMS_LISTS)
                    .then((data) => {
                        this.MinecraftItems = data;
                        this.loadingData = false;
                    })
                    .catch((e) => {
                        this.loadingData = true;
                        let self = this;
                        setTimeout(function () {
                            self.fetchItemsLists();
                        }, 2000);
                    });
            },
        }
    }
</script>

<style scoped>

</style>
