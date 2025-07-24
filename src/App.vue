<template>
  <AppPreloader v-if="!loaded" />
  <AppMap @loaded="loaded = true" />
</template>

<script setup lang="ts">
import { ref } from "vue";
import AppMap from "./components/AppMap.vue";
import AppPreloader from "./components/AppPreloader.vue";
const loaded = ref(false);
</script>
