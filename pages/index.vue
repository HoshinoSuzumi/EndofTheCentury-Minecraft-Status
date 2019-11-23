<template>
  <div>
    <NavBar/>
    <div class="main">
      <ContentArea title="服务器状态">

        <div class="etcs-container">

          <div v-for="(info, key) in infoList" :key="key" class="mdui-col-xs-12 mdui-col-sm-6 mdui-col-md-4">
            <div class="setting-block">
              <div class="setting-block-title">
                {{ info.title }}
              </div>
              <div class="setting-block-icon">
                <i class="mdui-icon material-icons">{{ info.icon }}</i>
              </div>
              <div class="setting-block-subtext" v-html="MQ_SERVER_INFO[info.field] || '-'"></div>
            </div>
          </div>

        </div>
      </ContentArea>

      <ContentArea title="在线玩家列表">

        <div class="etcs-container">

          <div v-if="MQ_SERVER_PLAYERS == null"><h3>无法获取玩家列表</h3></div>
          <div v-else-if="MQ_SERVER_PLAYERS.length === 0"><h3>当前没有玩家在线</h3></div>
          <div v-else v-for="player in MQ_SERVER_PLAYERS" class="mdui-col-xs-12 mdui-col-sm-6 mdui-col-md-4">
            <div class="etcs-player-label">
              {{ player }}
            </div>
          </div>

        </div>
      </ContentArea>

      <ContentArea title="服务器插件列表">

        <div class="etcs-container">

          <div v-if="MQ_SERVER_PLUGINS == null"><h3>无法获取插件列表</h3></div>
          <div v-else-if="MQ_SERVER_PLUGINS.length === 0"><h3>服务器没有启用任何插件</h3></div>
          <div v-else v-for="plugin in MQ_SERVER_PLUGINS" class="mdui-col-xs-12 mdui-col-sm-6 mdui-col-md-4">
            <div class="etcs-plugin-label">
              {{ plugin }}
            </div>
          </div>

        </div>
      </ContentArea>

    </div>
    <footer class="etcs-footer">
      <p>
        更新于: {{ UPDATED ? timestamp2Time(UPDATED) : '从未' }}
      </p>
    </footer>
  </div>
</template>

<script>
    import NavBar from "../components/NavBar";
    import ContentArea from "../components/ContentArea";

    // const MQ_API_BASE = 'https://mc.boxmoe.cn/api';
    const MQ_API_BASE = '/api';

    let intervalInfo, intervalPlayers;

    export default {
        name: "index",
        components: {ContentArea, NavBar},
        data() {
            return {
                infoList: [
                    {title: '服务器描述', icon: 'description', field: 'HostName'},
                    {title: '服务器状态', icon: 'cloud_queue', field: 'OnlineText'},
                    {title: '服务器地址', icon: 'location_searching', field: 'Host'},
                    // {title: '服务器 IP', icon: 'location_searching', field: 'HostIp'},
                    {title: '端口', icon: 'import_export', field: 'HostPort'},
                    {title: '平台', icon: 'desktop_windows', field: 'GameName'},
                    {title: '游戏类型', icon: 'games', field: 'GameType'},
                    {title: '兼容游戏版本', icon: 'build', field: 'Version'},
                    {title: '服务器核心', icon: 'donut_large', field: 'Software'},
                    {title: '在线玩家', icon: 'person', field: 'Players'},
                    {title: '玩家容量', icon: 'group', field: 'MaxPlayers'},
                    {title: '查询协议', icon: 'sync', field: 'QueryType'},
                    {title: '查询耗时', icon: 'access_time', field: 'Duration'},
                ],
                MQ_SERVER_INFO: {},
                MQ_SERVER_PLAYERS: null,
                MQ_SERVER_PLUGINS: null,
                UPDATED: null,
            }
        },
        methods: {
            fetchInfo() {
                this.$axios.get(MQ_API_BASE + '/GetSvrInfo')
                    .then((response) => {
                        response = response.data;
                        this.UPDATED = response.time;
                        this.MQ_SERVER_INFO = response.data;
                        if (typeof this.MQ_SERVER_INFO['Players'] === "number")
                            this.MQ_SERVER_INFO['Players'] = this.MQ_SERVER_INFO['Players'].toString();
                        this.MQ_SERVER_PLUGINS = response.data['Plugins'] === undefined ? null : response.data['Plugins'];
                    });
            },
            fetchPlayers() {
                this.$axios.get(MQ_API_BASE + '/GetSvrPlayers')
                    .then((response) => {
                        response = response.data;
                        this.UPDATED = response.time;
                        this.MQ_SERVER_PLAYERS = response.data;
                    });
            },
            timestamp2Time(timestamp) {
                if (timestamp / 1000000000 < 10)
                    timestamp *= 1000;
                let date = new Date(timestamp);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
                let Y = date.getFullYear() + '-';
                let M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
                let D = date.getDate() < 10 ? '0' + date.getDate() + ' ' : date.getDate() + ' ';
                let h = date.getHours() < 10 ? '0' + date.getHours() + ':' : date.getHours() + ':';
                let m = date.getMinutes() < 10 ? '0' + date.getMinutes() + ':' : date.getMinutes() + ':';
                let s = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds();
                return Y + M + D + h + m + s;
            },
        },
        mounted() {
            let self = this;
            intervalInfo = setInterval(function () {
                self.fetchInfo();
            }, 3000);
            intervalPlayers = setInterval(function () {
                self.fetchPlayers();
            }, 3000);
        },
    }
</script>

<style scoped>
  .main {
    width: 100%;
    top: 50px;
    padding-bottom: 5rem;
  }

  .etcs-container {
    display: flex;
    flex-wrap: wrap;
    border-radius: 10px;
    background: #FFFFFF;
    box-shadow: 0.5rem 0.875rem 2.375rem rgba(39, 44, 49, .06), 0.0625rem 0.1875rem 0.5rem rgba(39, 44, 49, .03);
    padding: 1.5rem 15px 15px;
  }

  .etcs-player-label,
  .etcs-plugin-label {
    display: flex;
    flex-wrap: wrap;
    border-radius: 10px;
    background: #FFFFFF;
    box-shadow: 0.5rem 0.875rem 2.375rem rgba(39, 44, 49, .06), 0.0625rem 0.1875rem 0.5rem rgba(39, 44, 49, .03);
    padding: 1.5rem 15px 15px;
    margin-bottom: 0.85rem;
    font-weight: 400;
  }
</style>
