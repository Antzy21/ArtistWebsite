<script setup lang="ts">
import { ref, defineProps, onMounted, onUnmounted, computed } from "vue";

const { images, negativeTransition = false, transitionStyle = "linear" } = defineProps<{
    images: string[],
    negativeTransition?: boolean;
    transitionStyle?: string;
}>();

const isMobileView = ref(window.innerWidth < 768);

const shiftWidth = computed(() => {
    return isMobileView.value ? 50 : 25;
});

const transitionDirection = computed(() => negativeTransition ? -1 : 1);

const startingIndex = ref(10 + 2 * -transitionDirection.value);

const repeatedImages = computed(() => {
    const repetitions = Math.ceil(16 / images.length); // Calculate minimum repetitions to reach 16
    return Array(repetitions).fill(images).flat().slice(0, 16); // Repeat and flatten the array
});

const indexShift = ref(0);
const isTransitioning = ref(true);

const transitionValue = computed(() => {
    return (-2*shiftWidth.value + (indexShift.value * shiftWidth.value)) * transitionDirection.value - shiftWidth.value * 6.5;
});

const nextSlide = () => {
    if (indexShift.value === images.length) {
        isTransitioning.value = false;
        indexShift.value = 0;
        setTimeout(() => {
            isTransitioning.value = true;
            indexShift.value ++;
        }, 20); // Allow DOM to update before transitioning
    } else {
        indexShift.value ++;
    }
};

let slideInterval: number;

onMounted(() => {
    const handleResize = () => {
        isMobileView.value = window.innerWidth < 768;
    };
    window.addEventListener("resize", handleResize);
    slideInterval = setInterval(nextSlide, 3000);
});

onUnmounted(() => {
    window.removeEventListener("resize", () => {});
    clearInterval(slideInterval);
});
</script>

<template>
    <div class="carousel-images"
        :style="{ transform: `translateX(${transitionValue}%)`, transition: isTransitioning ? `transform 3s ${transitionStyle}` : 'none' }">
        <img v-for="(image, index) in repeatedImages" :key="index" :src="image" :alt="'Image ' + (index + 1)"
            class="carousel-image" />
    </div>
</template>

<style scoped>
.carousel-images {
    display: flex;
}

.carousel-image {
    padding: 1rem 0.25rem;
    width: 50%;
    /* Ensure 4 images fit the width of the carousel */
    object-fit: cover;
    /* Maintain aspect ratio */
}

@media (min-width: 1024px) {
    .carousel-image {
        width: 25%;
        padding: 2rem 1rem;
    }
}
</style>