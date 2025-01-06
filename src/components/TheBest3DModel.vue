<script setup lang="ts">
import { defineProps, onMounted } from 'vue';
import type { Model3d } from '@/types';
const props = defineProps<Model3d>();  //пропсы


// обработка ошибки
const handleError = (e: Event) => {
    console.error('Ошибка загрузки модели:', e);
};

onMounted(() => {
    let metallic = document.querySelector("#metal");
    const modelViewerTexture1 = document.querySelector("model-viewer#modelView");
    modelViewerTexture1?.addEventListener("load", () => {
        const material = modelViewerTexture1.model.materials[0];
        const createAndApplyTextureOnline = async (channel: any, event: any) => {
            const texture = await modelViewerTexture1.createTexture(
                event
            );
            if (channel.includes("base") || channel.includes("metallic")) {
                material.pbrMetallicRoughness[channel].setTexture(texture);
            } else material[channel].setTexture(texture);
        };
        createAndApplyTextureOnline("baseColorTexture", props?.texture)
        createAndApplyTextureOnline("metallicRoughnessTexture", props?.metalic)
    });
})

</script>

<template>
    <div class="model-viewer-container">
        <!-- Cама моделька -->
        <model-viewer id="modelView" loading="eager"
            :style="`background-color: ${background}; height: ${height}px; width: ${width}px;`" :src="model"
            :camera-controls="true" :auto-rotate="true" @error="handleError" :ar="true">
        </model-viewer>
    </div>
</template>
