<template>
  <div class="w-full h-full" ref="container"></div>
</template>

<script setup lang="ts">
import { Map, addProtocol } from "maplibre-gl";
import type { MapGeoJSONFeature } from "maplibre-gl";
import { Protocol } from "pmtiles";
// @ts-ignore
import { style } from "@/data/style.js";
import { ref, onMounted, shallowRef, markRaw, watch } from "vue";
import type { Raw } from "vue";

const emit = defineEmits(["clickFeature", "loaded"]);
const container = ref<HTMLElement>();
const map = shallowRef<Raw<Map>>();
const selectedFeature = ref<MapGeoJSONFeature | null>(null);

let protocol = new Protocol();

addProtocol("pmtiles", protocol.tile);

onMounted(() => {
  map.value = markRaw(
    new Map({
      container: container.value!,
      style: style,
      center: [30.49354, 50.44423],
      zoom: 11,
      maxZoom: 15,
      minZoom: 10,
      maxBounds: [
        [30.11529, 50.16268],
        [30.95087, 50.65882],
      ],
    })
  );

  map.value.on("load", () => {
    emit("loaded");
  });
  map.value.on("click", () => {
    // map.value!.setFilter("highlighted", ["in", "id", ""]);
    selectedFeature.value = null;
  });
  map.value.on("click", "prom", (e) => {
    if (e.features?.length) {
      let feature = e.features[0];
      // map.value!.setFilter("highlighted", ["in", "id", feature.properties.id]);
      selectedFeature.value = feature;
    }
  });
});
</script>
