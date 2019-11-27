<template>
  <div>
    <ContentArea title="服务器卫星地图">
      <iframe class="etcs-webframe" id="map-frame" frameborder="0"
              :width="frameWidth"
              :height="frameHeight"
              :src="'http://etcs.redneno.me:8002/'"/>
    </ContentArea>
  </div>
</template>

<script>
  import ContentArea from "../components/ContentArea";

  export default {
    name: "etcs-map-online",
    components: {ContentArea},
    data() {
      return {
        frameWidth: '100%',
        frameHeight: '',
      }
    },
    methods: {
      updateFrameScale() {
        let innerWidth = window.innerWidth;
        let innerHeight = window.innerHeight;
        let frameWidth = document.querySelector('#map-frame').clientWidth;
        let scale = frameWidth / innerWidth;
        scale = 10 / 16;
        if (innerWidth > innerHeight) {
          this.frameHeight = innerHeight * scale;
        } else {
          this.frameHeight = innerWidth * scale;
        }
      },
    },
    mounted() {
      this.updateFrameScale();
    },
  }
</script>

<style scoped>
  .etcs-webframe {
    border-radius: 10px;
    background: #FFFFFF;
    box-shadow: 0.5rem 0.875rem 2.375rem rgba(39, 44, 49, .06), 0.0625rem 0.1875rem 0.5rem rgba(39, 44, 49, .03);
    transition: all .3s;
  }
</style>
