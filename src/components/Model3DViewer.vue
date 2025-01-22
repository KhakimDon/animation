<script setup lang="ts">
import { onMounted, ref } from "vue";
import type { Model3d } from "@/types";
const props = defineProps<Model3d>(); //пропсы

// обработка ошибки
const handleError = (e: Event) => {
  console.error("Ошибка загрузки модели:", e);
};
const load = ref(true);

async function onModelLoad(e: any) {
  const modelViewerTexture = e.target;
  const material = modelViewerTexture.model.materials[0];
  const createAndApplyTextureOnline = async (channel: any, event: any) => {
    const texture = await modelViewerTexture?.createTexture(event);
    if (channel.includes("base") || channel.includes("metallic")) {
      material?.pbrMetallicRoughness[channel].setTexture(texture);
    } else material[channel].setTexture(texture);
  };
  await createAndApplyTextureOnline("baseColorTexture", props?.texture);
  await createAndApplyTextureOnline("metallicRoughnessTexture", props?.metalic);

  setTimeout(() => (load.value = false), 100);

  modelViewerTexture.environmentImage = "";
}
</script>

<template>
  <model-viewer
    @load="onModelLoad"
    style="width: 100%; height: 100vh"
    id="modelView"
    :src="model"
    :camera-controls="true"
    :auto-rotate="true"
    @error="handleError"
    min-camera-orbit="auto auto 3m"
    max-camera-orbit="auto auto 16m"
    :ar="true"
  >
  </model-viewer>
  <div :class="{ 'preloader-hidden': !load === true }" class="preloader">
    loading...
  </div>
</template>

<style>
.preloader {
  display: flex;
  justify-content: center;
  color: white;
  font-size: 23px;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  align-items: center;
  height: 100%;
  width: 100%;
  position: absolute;
  left: 0;
  backdrop-filter: blur(30px);
  top: 0;
  transition-duration: 300ms;
  background: #171717bf;
}
.preloader-hidden {
  opacity: 0;
  pointer-events: none;
}
</style>