<script setup>
import BackDrop from "./components/BackDrop.vue";
import HeroSection from "./components/HeroSection.vue";
import MainSection from "./components/MainSection.vue";
</script>

<template>
  <BackDrop />
  <main>
    <MainSection />
    <HeroSection />
  </main>
</template>

<style scoped>
@media only screen and (max-width: 1023px) {
  main {
    width: 100%;
    margin: 0 auto;
    padding: 10px;
    display: flex;
    position: relative;
    align-items: center;
    gap: 10px;
  }
}
@media only screen and (min-width: 1024px) {
  main {
    width: 80%;
    margin: 0 auto;
    padding: 10px;
    height: 100vh;
    display: flex;
    position: relative;
    align-items: center;
    gap: 10px;
    justify-content: space-between;
  }
}
</style>
