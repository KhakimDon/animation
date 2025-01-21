<script setup lang="ts">
import { ref } from 'vue';
import Model3DViewer from './components/Model3DViewer.vue';
import type { Model3d } from './types';

// настройки 3д модели 
const options3d = ref<Model3d>({
  model: 'https://3d-project-waxpeer.netlify.app/knife_bayonet.glb', //Ссылка на модельz
  texture: 'https://images.waxpeer.com/screenshot/texture/40863259329_texture_auto_1734857014018.png',                                   //Ссылка на текстуру
})
</script>

<template>
  <div class="container">
    <Model3DViewer v-bind="options3d" />
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