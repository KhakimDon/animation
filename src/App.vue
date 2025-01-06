<script setup lang="ts">
import { ref } from 'vue';
import TheBest3DModel from './components/TheBest3DModel.vue';
import type { Model3d } from './types';

// настройки 3д модели 
const options3d = ref<Model3d>({
  model: 'https://3d-project-waxpeer.netlify.app/karambit.glb', //Ссылка на модель
  texture: 'https://images.waxpeer.com/screenshot/texture/40863259329_texture_auto_1734857014018.png',                                   //Ссылка на текстуру
  metalic:'https://images.waxpeer.com/screenshot/metalness/40863259329_metalness_auto_1734857014018.png',                                    //Металика если есть укажем ссылку, если нет то null
  height: 500,                                              //Высота в пикселях
  width: 500,                                               //Ширина в пикселях
  background: "green"                                      //Ширина в пикселях
})

</script>

<template>
  <div class="model-wrapper">
    <TheBest3DModel v-bind="options3d" />
  </div>
</template>
