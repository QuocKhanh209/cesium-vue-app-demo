<template>
  <div ref="containerRef" class="viewer-container"></div>
</template>

<script setup>
import { onMounted, onBeforeUnmount, ref } from "vue";
import * as Cesium from "cesium";

const containerRef = ref(null);
let viewer = null;

onMounted(() => {
  viewer = new Cesium.Viewer(containerRef.value, {
    baseLayerPicker: false,
    geocoder: false,
    homeButton: false,
    infoBox: false,
    selectionIndicator: false,
    sceneModePicker: false,
    timeline: false,
    navigationHelpButton: false,
    showCredit: false,
    animation: false,
    baseLayer: new Cesium.ImageryLayer(
      new Cesium.OpenStreetMapImageryProvider({
        url: "https://tile.openstreetmap.org/",
        enablePickFeatures: false,
      })
    ),
    creditContainer: document.createElement("none"),
    fullscreenButton: false,
    maximumRenderTimeChange: Infinity,
    useBrowserRecommendedResolution: false,
    contextOptions: {
      webgl: {
        alpha: false,
        depth: true,
        stencil: false,
        antialias: true,
        premultipliedAlpha: true,
        preserveDrawingBuffer: false, // Đặt false để tăng hiệu suất
        failIfMajorPerformanceCaveat: false,
        powerPreference: "high-performance", // Ưu tiên GPU mạnh
      },
    },
    terrain: new Cesium.Terrain(
      Cesium.CesiumTerrainProvider.fromUrl(
        "http://localhost:8060/tilesets/terrainfix"
      )
    ),
  });

  // Fly to default view
  viewer.scene.camera.flyHome(0);
});

onBeforeUnmount(() => {
  if (viewer) {
    viewer.destroy();
    viewer = null;
  }
});
</script>

<style scoped>
.viewer-container {
  position: fixed;
  inset: 0;
}
</style>
