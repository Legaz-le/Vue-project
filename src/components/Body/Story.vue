<script setup>
import { ref } from "vue";
import AnimatedTitle from "../Reusable/AnimatedTitle.vue";
import gsap from "gsap";
import Button from "../Reusable/Button.vue"

const frameRef = ref(null);
const handleMouseLeave = () => {
  const element = frameRef.value;

  gsap.to(element, {
    duration: 0.3,
    rotateX: 0,
    rotateY: 0,
    ease: "power1.inOut",
  });
};
const handleMouseMove = (e) => {
  const { clientX, clientY } = e;
  const element = frameRef.value;

  if (!element) return;

  const react = element.getBoundingClientRect();
  const x = clientX - react.left;
  const y = clientY - react.top;

  const centerX = react.width / 2;
  const centerY = react.height / 2;

  const rotateX = ((y - centerY) / centerY) * -10;
  const rotateY = ((x - centerX) / centerX) * 10;

  gsap.to(element, {
    duration: 0.3,
    rotateX,
    rotateY,
    transformPerspective: 500,
    ease: "power1.inOut",
  });
};
</script>

<template lang="">
  <section id="story" class="bg-black min-h-dvh w-screen text-blue-50">
    <div class="flex size-full flex-col items-center py-10 pb-24">
      <p class="font-general text-sm uppercase md:text-[10px]">
        the multiversal ip world
      </p>
      <div class="relative size-full">
        <AnimatedTitle
          :title="`The st<b>o</b>ry of <br /> a hidden real<b>m</b>`"
          sectionId="#story"
          containerClass="mt-5 pointer-events-none mix-blend-difference relative z-10"
        />
        <div class="story-img-container">
          <div class="story-img-mask">
            <div class="story-img-content">
              <img
                ref="frameRef"
                @mouseleave="handleMouseLeave"
                @mouseup="handleMouseLeave"
                @mouseenter="handleMouseLeave"
                @mousemove="handleMouseMove"
                src="/img/entrance.webp"
                alt="etrance"
                class="object-contain"
              />
            </div>
          </div>
          <svg
            className="invisible absolute size-0"
            xmlns="http://www.w3.org/2000/svg"
          >
            <defs>
              <filter id="flt_tag">
                <feGaussianBlur
                  in="SourceGraphic"
                  stdDeviation="8"
                  result="blur"
                />
                <feColorMatrix
                  in="blur"
                  mode="matrix"
                  values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -9"
                  result="flt_tag"
                />
                <feComposite in="SourceGraphic" in2="flt_tag" operator="atop" />
              </filter>
            </defs>
          </svg>
        </div>
      </div>
      <div
        class="-mt-80 flex w-full justify-center md:-mt-64 md:me-44 md:justify-end"
      >
        <div class="flex h-full w-fit flex-col items-center md:items-start">
          <p class="mt-3 max-w-sm text-center font-circular-web text-violet-50 md:text-start">
            Where realms converge, lies Zentry and the boundless pillar.
            Discover its secrets and shape your fate amidst infinite
            opportunities.
          </p>
          <Button id="realm-button" title="discover prologue" containerClass="mt-5" />
        </div>
      </div>
    </div>
  </section>
</template>
