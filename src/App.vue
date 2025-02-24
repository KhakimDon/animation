<script setup lang="ts">
import { ref } from 'vue';
// import Model3DViewer from './components/Model3DViewer.vue';
import KnifeAnimation from './components/KnifeAnimation.vue';
import type { Model3d } from './types';

// настройки 3д модели 
const options3d = ref<Model3d>({
  model: 'http://vermillion-unicorn-ec51323232322c9.netlify.app/karamanimation.glb', //Ссылка на модельz
  texture: 'https://cdn.skinport.com/cdn-cgi/image/fit=pad,format=avif,quality=85,background=transparent/3d-viewer/textures/408598615/material0_color_weapon_knife_karambit.png',                                   //Ссылка на текстуру
  metalic: 'https://cdn.skinport.com/cdn-cgi/image/fit=pad,format=avif,quality=85,background=transparent/3d-viewer/textures/408598615/material0_metal_weapon_knife_karambit.png',                                   //Ссылка на текстуру
  sun: '/khakim11.hdr',                                   //Ссылка на текстуру
})
</script>

<template>
  <div class="container">
    <KnifeAnimation v-bind="options3d" />
  </div>
</template>


<style scoped>
.container {
  height: 100vh;
  width: 100%;
  background: #171717;
  position: relative;
}
</style>