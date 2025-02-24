<script setup lang="ts">
import { onMounted, ref } from "vue";
import type { Model3d } from "@/types";
import { log } from "console";
const props = defineProps<Model3d>(); //пропсы
const sun = ref("");

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
      await material?.pbrMetallicRoughness[channel].setTexture(texture);
    } else await material[channel].setTexture(texture);
  };
  await createAndApplyTextureOnline("metallicRoughnessTexture", props?.metalic);
  await createAndApplyTextureOnline("baseColorTexture", props?.texture);

  

  setTimeout(() => (load.value = false), 100);

  sun.value = props?.sun;
  console.log(sun.value);
  
  modelViewerTexture.environmentImage = "";
}
</script>

<template>
  <model-viewer
    @load="onModelLoad"
    style="width: 100%; height: 100vh"
    id="modelView"
    :src="model"
    camera-controls
    occlusion="true"
    :environment-image="sun"
    @error="handleError"
    camera-orbit="0deg 90deg 105%"
    orbit-sensitivity="1.5"
    shadow-intensity="1"
    min-camera-orbit="auto auto 3m"
    max-camera-orbit="auto auto 16m"
    ar
    ar-modes="webxr scene-viewer quick-look"
    :exposure="2"
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
