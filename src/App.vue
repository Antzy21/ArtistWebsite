<script setup lang="ts">
import Carousel from './Carousel.vue';
import { ref } from 'vue';
import appSettings from './config/appSettings.dev.json';

const carousel_images_1: string[] = appSettings.carouselImages[0];
const carousel_images_2: string[] = appSettings.carouselImages[1];

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
    <img alt="Artist image" class="logo" :src="artistImage" width="250" height="250" />
  </header>

  <main>
    <Carousel :images="carousel_images_1"></Carousel>
    <h2>"Quote from a customer that is very positive" - Customer Name</h2>
    <Carousel :images="carousel_images_2" :negativeTransition="false"></Carousel>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

.logo {
  display: block;
  border: 5px solid rgba(0, 0, 0, 0.1); /* Faded border */
  border-radius: 10px; /* Optional: Add rounded corners */
}

.header-text {
  line-height: 2;
  white-space: normal;
  padding: 0 3rem;
}

@media (min-width: 1024px) {
  header {
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
