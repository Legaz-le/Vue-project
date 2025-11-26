<script setup>
import { watch, onMounted } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import AnimatedTitle from "./Reusable/AnimatedTitle.vue";

gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
  const clipAnimation = gsap.timeline({
    scrollTrigger: {
      trigger: "#clip",
      start: "center center",
      end: "+=800 center",
      scrub: 0.5,
      pin: true,
      pinSpacing: true,
    },
  });

  clipAnimation.to(".mask-clip-path", {
    width: "100vw",
    height: "100vh",
    borderRadius: 0,
  });
});
</script>

<template lang="">
  <div id="about" class="min-h-screen w-screen">
    <div class="relative mb-8 mt-36 flex flex-col items-center gap-5">
      <p class="font-general text-sm uppercase md:text-[10px]">
        Welcome to Zentry
      </p>
      <AnimatedTitle
        :title="`Disc<b>o</b>ver the world's <br /><b>l</b>argest shared adventure`"
        containerClass="mt-5 !text-black text-center"
      />
      <div class="about-subtext">
        <p>The Game of Games begins-your life, now an epic MMORPG</p>
        <p>Zentry unites every player from countless games and platforms</p>
      </div>
    </div>
    <div id="clip" class="h-dvh w-screen relative">
      <div class="mask-clip-path about-image">
        <img
          src="/img/about.webp"
          alt="Background"
          class="absolute left-0 top-0 size-full object-cover"
        />
      </div>
    </div>
  </div>
</template>
