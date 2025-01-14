<script setup lang="ts">
import { onMounted, ref } from "vue";
import type { Model3d } from "@/types";
const props = defineProps<Model3d>(); //пропсы

// обработка ошибки
const handleError = (e: Event) => {
  console.error("Ошибка загрузки модели:", e);
};
const load = ref(true);

function onModelLoad(e: any) {
  const modelViewerTexture = e.target;
  const material = modelViewerTexture.model.materials[0];
  const createAndApplyTextureOnline = async (channel: any, event: any) => {
    const texture = await modelViewerTexture.createTexture(event);
    if (channel.includes("base") || channel.includes("metallic")) {
      material.pbrMetallicRoughness[channel].setTexture(texture);
    } else material[channel].setTexture(texture);
  };
  createAndApplyTextureOnline("baseColorTexture", props?.texture);
  createAndApplyTextureOnline("metallicRoughnessTexture", props?.metalic);

  setTimeout(() => (load.value = false), 1000);
}
</script>

<template>
  <h1 style="color: white">{{ load }}</h1>
  <model-viewer
    @load="onModelLoad"
    style="width: 100%; height: 100vh"
    id="modelView"
    :src="model"
    :camera-controls="true"
    :auto-rotate="true"
    @error="handleError"
    camera-orbit="70deg 75deg 105%"
    :ar="true"
  >
  </model-viewer>
  <div :class="{ 'preloader-hidden': !load === true }" class="preloader"></div>
</template>

<style>
.preloader {
  height: 100%;
  width: 100%;
  position: absolute;
  left: 0;
  backdrop-filter: blur(5px);
  top: 0;
  transition-duration: 300ms;
  background: rgba(0, 0, 0, 0.209);
}
.preloader-hidden {
  opacity: 0;
  pointer-events: none;
}
</style>