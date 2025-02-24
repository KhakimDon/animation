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

const startAnim = (event: Event) => {
  const modelViewer = document.querySelector("#modelView");
  modelViewer.animationName = event.target.value;

  document.querySelectorAll("button").forEach((e) => (e.disabled = true));
  setTimeout(() => {
    modelViewer.animationName = "idle1";
    document.querySelectorAll("button").forEach((e) => (e.disabled = false));
  }, 900);
};
</script>

<template>
  <div class="container">
    <button value="lookat01" @click="startAnim">
      Посмотреть скин (3 разных варианта, нажимите 3 раза)
    </button>
    <button value="draw" @click="startAnim">достать ножик</button>
    <button value="idle2" @click="startAnim">При движении</button>
    <button value="heavy_backstab" @click="startAnim">Критичный урон</button>
    <button value="heavy_hit1" @click="startAnim">Критичный урон 2</button>
    <button value="heavy_miss1" @click="startAnim">
      Критичный урон промах
    </button>
    <button value="light_hit1" @click="startAnim">простой урон</button>
    <button value="light_hit2" @click="startAnim">простой урон 2</button>
    <button value="light_miss1" @click="startAnim">еще какойто</button>
  </div>
  <!-- 
light_miss1
light_miss2
lookat01 -->
  <model-viewer
    @load="onModelLoad"
    style="width: 100vw; height: 100vh; pointer-events: none"
    id="modelView"
    :src="model"
    animation-name="idle1"
    autoplay
    camera-controls
    occlusion="true"
    :environment-image="sun"
    @error="handleError"
    camera-orbit="0deg 90deg 105%"
    orbit-sensitivity="1.5"
    shadow-intensity="1"
    min-camera-orbit="auto auto 40m"
    max-camera-orbit="auto auto 100m"
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
.container {
  position: absolute;
  width: 40%;
  height: 150px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
</style>
