<script setup lang="ts">
import { ref, defineProps, onMounted, onUnmounted, computed } from "vue";

const { images, negativeTransition = true } = defineProps<{
    images: string[],
    negativeTransition?: boolean;
}>();

const transitionDirection = computed(() => {
    return negativeTransition ? -1 : 1;
});

const startingIndex = computed(() => images.length * (transitionDirection.value));

const currentIndex = ref(startingIndex.value);
const isTransitioning = ref(true);

const transitionValue = computed(() => {
    return -250 + ((currentIndex.value / 4) * 100) * transitionDirection.value;
});

const nextSlide = () => {
    if (currentIndex.value % images.length == 0) {
        isTransitioning.value = false;
        currentIndex.value = startingIndex.value
        setTimeout(() => {
            isTransitioning.value = true; // Re-enable transition
            currentIndex.value++;
        }, 10); // Allow DOM to update before transitioning
    } else {
        currentIndex.value++;
    }
};

const repeatedImages = computed(() => {
    const repetitions = Math.ceil(16 / images.length); // Calculate minimum repetitions to reach 16
    return Array(repetitions).fill(images).flat(); // Repeat and flatten the array
});

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
    width: 25%; /* Ensure 4 images fit the width of the carousel */
    object-fit: cover; /* Maintain aspect ratio */
}
</style>