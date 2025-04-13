<script setup lang="ts">
import Carousel from './Carousel.vue';
import { ref } from 'vue';
import appSettings from './config/appSettings.dev.json';

const carousel_images_1: string[] = appSettings.carouselImages[0] ?? [];
const carousel_images_2: string[] = appSettings.carouselImages[1] ?? [];
const carousel_images_3: string[] = appSettings.carouselImages[2] ?? [];

const artistName = ref(appSettings.artistName);
const artistImage = appSettings.artistImage;
const subtitle = ref(appSettings.subtitle);

</script>

<template>
  <header>
    <div class="header-text">
      <h1>{{ artistName }}</h1>
      <h2>{{ subtitle }}</h2>
    </div>
    <img alt="Artist image" class="logo" :src="artistImage" width="20%" />
  </header>

  <main>
    <h2>Carousel with 2 images</h2>
    <Carousel :images="carousel_images_1"></Carousel>
    <h2>Reverse Carousel with 2 images</h2>
    <Carousel :images="carousel_images_1" negativeTransition></Carousel>
    <h2>Carousel with transition: "ease-in-out"</h2>
    <Carousel :images="carousel_images_1" transitionStyle="ease-in-out"></Carousel>
    <h2>Carousel with 3 images</h2>
    <Carousel :images="carousel_images_2"></Carousel>
    <h2>Reverse Carousel with 3 images</h2>
    <Carousel :images="carousel_images_2" negativeTransition></Carousel>
    <h2>Carousel with 4 images</h2>
    <Carousel :images="carousel_images_3"></Carousel>
    <h2>Reverse Carousel with 4 images</h2>
    <Carousel :images="carousel_images_3" negativeTransition></Carousel>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo {
  display: block;
  border: 5px solid rgba(0, 0, 0, 0.1);
  border-radius: 20px;
}

.header-text {
  line-height: 2;
  padding: 0 2rem;
  white-space: normal;
}

@media (min-width: 1024px) {
  h2 {
    margin: 1rem 0;
  }

  header {
    padding: 0 3rem;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
