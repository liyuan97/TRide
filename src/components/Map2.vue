<template>
  <div id="map-layout">
    <div ref="mapContainer" class="map-container"></div>
  </div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

export default {
  props: ['modelValue'],

  data() {
    return {
      map: null,
      markerLayer: null // 用于保存标记的图层
    };
  },

  mounted() {
    this.initMap();
  },

  methods: {
    initMap() {
      const { lng, lat, zoom } = this.modelValue;

      // 初始化地图
      this.map = L.map(this.$refs.mapContainer).setView([lat, lng], zoom);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '© OpenStreetMap contributors'
      }).addTo(this.map);

      // 添加定位控件
      L.control.locate().addTo(this.map);

      // 添加车辆标记
      this.addCarMarkers();

      // 地图事件监听
      this.map.on('moveend', this.updateLocation);
    },

    addCarMarkers() {
      const cars = this.cars; // 假设 `cars` 是一个包含车辆信息的数组
      this.markerLayer = L.layerGroup().addTo(this.map); // 创建一个新的图层来保存标记

      cars.forEach(car => {
        const { coordinates } = car;
        L.marker([coordinates[1], coordinates[0]]).addTo(this.markerLayer);
      });
    },

    updateLocation() {
      // 更新位置逻辑，如逆地理编码等
    }
  },

  watch: {
    modelValue: {
      deep: true,
      handler(newVal) {
        if (this.map) {
          const { lng, lat, zoom } = newVal;
          this.map.setView(new L.LatLng(lat, lng), zoom);
        }
      }
    }
  },

  beforeDestroy() {
    if (this.map) {
      this.map.remove();
    }
  }
};
</script>

<style>
.map-container {
  height: 100vh;
  width: 100%;
}
</style>
