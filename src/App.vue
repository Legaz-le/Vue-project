<script setup>
import Hero from "./components/Body/Hero.vue";
import About from "./components/Body/About.vue";
import Navbar from "./components/Body/Navbar.vue";
import Features from "./components/Body/Features.vue";
</script>

<template>
  <div class="relative min-h-screen w-screen overflow-x-hidden">
    <Navbar />
    <Hero />
    <About />
    <Features />
  </div>
</template>
