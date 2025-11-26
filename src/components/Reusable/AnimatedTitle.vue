<script setup>
import { ref, onMounted, computed, onBeforeUnmount } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const props = defineProps({
  title: String,
  containerClass: String,
});

const containRef = ref(null);
const lines = computed(() => {
  return props.title.split("<br />").map((line) => line.split(" "));
});

onMounted(() => {
  const ctx = gsap.context(() => {
    const titleAnimation = gsap.timeline({
      scrollTrigger: {
        trigger: containRef.value,
        start: "100 bottom",
        end: "center center",
        toggleActions: "play none none reverse",
      },
    });

    titleAnimation.to(containRef.value.querySelectorAll(".animated-word"), {
      opacity: 1,
      transform: "translate3d(0,0,0) rotateY(0deg) rotateX(0deg)",
      ease: "power2.inOut",
      stagger: 0.02,
    });
  }, containRef);
  onBeforeUnmount(() => {
    ctx.revert();
  });
});
</script>

<template lang="">
  <div ref="containRef" class="animated-title" :class="containerClass">
    <div
      v-for="(line, i) in lines"
      :key="i"
      class="flex-center max-w-full flex-wrap gap-2 px-10 md:gap-3"
    >
      <span
        v-for="(char, c) in line"
        :key="c"
        class="animated-word"
        v-html="char"
      ></span>
    </div>
  </div>
</template>
