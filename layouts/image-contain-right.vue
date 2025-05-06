<!--
  image-contain-right.vue
  A custom layout for Slidev that positions an image on the right side with background-size: contain by default
-->
<template>
    <div class="my-auto w-full grid grid-cols-2 2-full h-full">
        <div class="slidev-layout default">
            <slot />
        </div>
        <div class="image-container" :style="imageContainerStyle"></div>
    </div>
</template>

<script setup>
import { computed } from "vue";
function resolveAssetUrl(url) {
    return url.startsWith("/")
        ? `${import.meta.env.BASE_URL}${url.slice(1)}`
        : url;
}
// Define props with defaults
const props = defineProps({
    image: {
        type: String,
        required: true,
    },
    backgroundSize: {
        type: String,
        default: "contain", // Default is 'contain' instead of 'cover' as in the original
    },
    backgroundPosition: {
        type: String,
        default: "center",
    },
    backgroundRepeat: {
        type: String,
        default: "no-repeat",
    },
});

// Compute the style for the image container
const imageContainerStyle = computed(() => {
    const resolvedImageUrl = resolveAssetUrl(props.image);

    return {
        backgroundImage: `url(${resolvedImageUrl})`,
        backgroundSize: props.backgroundSize,
        backgroundPosition: props.backgroundPosition,
        backgroundRepeat: props.backgroundRepeat,
    };
});
</script>

<style>
.image-contain-right .image-container {
    height: 100%;
    width: 100%;
}
</style>
