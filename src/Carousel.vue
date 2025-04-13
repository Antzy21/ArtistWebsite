<script setup lang="ts">
import { ref, defineProps, onMounted, onUnmounted, computed } from "vue";

const { images, negativeTransition = false } = defineProps<{
    images: string[],
    negativeTransition?: boolean;
}>();

const transitionDirection = computed(() => negativeTransition ? -1 : 1);

const startingIndex = ref(10 + 2 * -transitionDirection.value);

const repeatedImages = computed(() => {
    const repetitions = Math.ceil(16 / images.length); // Calculate minimum repetitions to reach 16
    return Array(repetitions).fill(images).flat().slice(0, 16); // Repeat and flatten the array
});

const indexShift = ref(0);
const isTransitioning = ref(true);

const transitionValue = computed(() => {
    return (-50 + (indexShift.value * 25)) * transitionDirection.value - 150;
});

const nextSlide = () => {
    if (indexShift.value === images.length) {
        isTransitioning.value = false;
        indexShift.value = 0;
        setTimeout(() => {
            isTransitioning.value = true;
            indexShift.value ++;
        }, 50); // Allow DOM to update before transitioning
    } else {
        indexShift.value ++;
    }
};

let slideInterval: number;

onMounted(() => {
    slideInterval = setInterval(nextSlide, 3000);
});

onUnmounted(() => {
    clearInterval(slideInterval);
});
</script>

<template>
    <div class="carousel-images"
        :style="{ transform: `translateX(${transitionValue}%)`, transition: isTransitioning ? 'transform 3s ease-in-out' : 'none' }">
        <img v-for="(image, index) in repeatedImages" :key="index" :src="image" :alt="'Image ' + (index + 1)"
            class="carousel-image" />
    </div>
</template>

<style scoped>
.carousel-images {
    display: flex;
}

.carousel-image {
    padding: 4rem 1rem;
    width: 25%;
    /* Ensure 4 images fit the width of the carousel */
    object-fit: cover;
    /* Maintain aspect ratio */
}
</style>