<script setup>
import { ref, watch } from "vue";
import Button from "./Reusable/Button.vue";
import { Icon } from "@iconify/vue";
import { useWindowScroll } from "@vueuse/core";
import gsap from "gsap";

const navContainerRef = ref(null);
const audioElementRef = ref(null);
const loading = ref(false);
const indicatorActive = ref(false);
const lastScroll = ref(0);
const visibleScroll = ref(true);
const navItems = ["Nexus", "Vault", "Prologue", "About", "Contact"];

const { y: currentScrollY } = useWindowScroll();

watch(currentScrollY, (value) => {
  if ((value === 0)) {
    visibleScroll.value = true;
    navContainerRef.value.classList.remove("floating-nav");
  } else if (value > lastScroll.value) {
    visibleScroll.value = false;
    navContainerRef.value.classList.add("floating-nav");
  } else if (value < lastScroll.value)  {
    visibleScroll.value = true;
    navContainerRef.value.classList.add("floating-nav");
  }
  
  lastScroll.value = value;
});

watch(visibleScroll, (isVisible) => {
    gsap.to(navContainerRef.value, {
        y:isVisible ? 0 : -100,
        opacity: isVisible ? 1 : 0,
        duration: 0.2,
    })
})

const toggleAudioIndicator = () => {
  loading.value = !loading.value;
  indicatorActive.value = !indicatorActive.value;
};

watch(loading, (newVal) => {
  if (!audioElementRef.value) return;

  if (newVal) {
    audioElementRef.value.play();
  } else {
    audioElementRef.value.pause();
  }
});
</script>

<template lang="">
  <div
    ref="navContainerRef"
    class="fixed inset-x-0 top-4 z-50 h-16 border-none transition-all duration-700 sm:inset-x-6"
  >
    <header class="absolute top-1/2 w-full -translate-y-1/2">
      <nav class="flex size-full items-center justify-between p-4">
        <div class="flex items-center gap-7">
          <img src="/img/logo.png" alt="logo" class="w-10" />
          <Button
            id="product-button"
            title="Products"
            rightIcon=""
            containerClass="bg-blue-50 md:flex hidden items-center justify-center gap-1"
            ><template #leftIcon>
              <Icon
                icon="iconamoon:arrow-top-right-2-bold"
                width="24"
                height="24"
              /> </template
          ></Button>
        </div>
        <div class="flex h-full items-center">
          <div class="hidden md:block">
            <a
              v-for="item in navItems"
              :key="item"
              :href="`#${item.toLowerCase()}`"
              v-html="item"
              class="nav-hover-btn"
            ></a>
          </div>
          <button
            @click="toggleAudioIndicator"
            class="ml-10 flex items-center space-x-0.5"
          >
            <audio
              ref="audioElementRef"
              class="hidden"
              src="/audio/loop.mp3"
              loop
            />
            <div
              v-for="n in 4"
              :key="n"
              class="indicator-line"
              :class="{ active: indicatorActive }"
              :style="{ animationDelay: `${n * 0.1}s` }"
            />
          </button>
        </div>
      </nav>
    </header>
  </div>
</template>
