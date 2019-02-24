<template>
  <div
    class="map"
    ref="map"
  />
</template>

<script>
import { loadModules, loadCss } from 'esri-loader';

export default {
  data: () => ({
    classes: {
      Map: null,
      SceneView: null,
    },
    map: null,
    view: null,
  }),

  async created() {
    await this.getModules();
    this.mountMap();
  },

  beforeUpdate() {
    this.destroy();
  },

  beforeDestroy() {
    this.destroy();
  },

  methods: {
    async getModules() {
      await loadCss('https://js.arcgis.com/4.9/esri/css/main.css');
      const [
        Map,
        SceneView,
      ] = await loadModules([
        'esri/Map',
        'esri/views/SceneView',
      ], {
        url: 'https://js.arcgis.com/4.9/',
      });

      this.classes.Map = Map;
      this.classes.SceneView = SceneView;
    },
    mountMap() {
      this.map = new this.classes.Map({
        basemap: 'osm',
      });

      this.view = new this.classes.SceneView({
        map: this.map,
        container: this.$refs.map,
        camera: {
          position: {
            latitude: -20.170875,
            longitude: 119.093041,
            z: 5000000,
          },
          tilt: 20,
        },
      });
    },
    destroy() {
      this.classes = this.$options.data.classes;
      this.map = this.$options.data.map;
      this.view = this.$options.data.view;
    },
  },
};
</script>

<style lang="scss" scoped>
.map {
  bottom: 0;
  left: 0;
  position: fixed;
  right: 0;
  top: 0;
}
</style>
