<script setup>
import { ref, computed, watch, nextTick, onMounted } from "vue";
import Button from "../Reusable/Button.vue";
import { Icon } from "@iconify/vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const currentIndex = ref(1);
const hasClicked = ref(false);
const isLoading = ref(true);
const loadedVideo = ref(0);

const totalVideo = 4;
const nextVideo = ref(null);
const currentVideo = ref(null);

const handleVideoLoad = () => {
  loadedVideo.value++;
};

const upcomingVideo = computed(() => (currentIndex.value % totalVideo) + 1);

const handleMiniVidClick = () => {
  hasClicked.value = true;

  currentIndex.value = upcomingVideo.value;
};

const videoSrc = (index) => `/videos/hero-${index}.mp4`;

watch(() => {
  if (loadedVideo.value === totalVideo - 1) {
    isLoading.value = false;
  }
}, [loadedVideo]);

watch(currentIndex, async () => {
  if (!hasClicked.value) return;

  await nextTick();

  gsap.set("#next-video", {
    visibility: "hidden",
    scale: 0,
    width: "64px",
    height: "64px",
  });

  gsap.set("#current-video", {
    scale: 1,
  });

  gsap.set("#next-video", { visibility: "visible" });

  gsap.to("#next-video", {
    transformOrigin: "center center",
    scale: 1,
    width: "100%",
    height: "100%",
    duration: 1,
    ease: "power1.inOut",
    onStart: () => nextVideo.value.play(),
  });

  gsap.from("#current-video", {
    transformOrigin: "center  center",
    scale: 0,
    duration: 0.5,
    ease: "power1.inOut",
  });
});

onMounted(() => {
  gsap.set("#video-frame", {
    clipPath: "polygon(14% 0, 72% 0, 88% 90%, 0 95%)",
    borderRadius: "0% 0% 40% 10%",
  });
  gsap.from("#video-frame", {
    clipPath: "polygon(0% 0, 100% 0, 100% 100%, 0 100%)",
    borderRadius: "0 0 0 0",
    ease: "power1.inOut",
    scrollTrigger: {
      trigger: "#video-frame",
      start: "center center",
      end: "bottom center",
      scrub: true,
    },
  });
});
</script>

<template lang="">
  <div class="relative h-dvh w-screen overflow-x-hidden">
  <div
    v-if="isLoading"
    class="flex-center absolute z-[100] h-dvh w-screen overflow-hidden bg-violet-50"
  >
    <div class="three-body">
      <div class="three-body__dot" />
      <div class="three-body__dot" />
      <div class="three-body__dot" />
    </div>
  </div>
  <div
    id="video-frame"
    class="relative z-10 h-dvh w-screen overflow-hidden rounded-lg bg-blue-75"
  >
    <div>
      <div
        class="mask-clip-path absolute-center absolute z-50 size-64 cursor-pointer overflow-hidden rounded-lg"
      >
        <div
          @click="handleMiniVidClick"
          class="origin-center scale-50 opacity-0 transition-all duration-500 ease-in hover:scale-100 hover:opacity-100"
        >
          <video
            :src="videoSrc(upcomingVideo)"
            ref="currentVideo"
            loop
            muted
            id="current-video"
            class="size-64 origin-center scale-150 object-cover object-center"
            @loadeddata="handleVideoLoad"
          ></video>
        </div>
      </div>
      <video
        ref="nextVideo"
        :src="videoSrc(currentIndex)"
        loop
        muted
        id="next-video"
        class="absolute-center invisible absolute z-20 size-64 object-cover object-center"
        @loadeddata="handleVideoLoad"
      ></video>
      <video
        :src="videoSrc(currentIndex)"
        loop
        muted
        class="absolute left-0 top-0 size-full object-cover object-center"
        @loadeddata="handleVideoLoad"
      ></video>
    </div>
    <h1
      class="special-font hero-heading absolute bottom-5 right-5 z-40 text-blue-75"
    >
      G<b>a</b>ming
    </h1>
    <div class="absolute left-0 top-0 z-40 size-full">
      <div class="mt-24 px-5 sm:px-10">
        <h1 class="special-font hero-heading text-blue-100">redefi<b>n</b>e</h1>
        <p class="mb-5 max-w-64 font-robert-regular text-blue-100 ">
          Enter MetaGame layer <br />
          Unleash the Play Economy
        </p>
        <Button
          id="watch-trailer"
          title="Watch Trailer"
          containerClass="!bg-yellow-300 flex-center gap-1"
          ><template #leftIcon>
            <Icon
              icon="iconamoon:arrow-top-right-2-bold"
              width="24"
              height="24"
            /> </template
        ></Button>
      </div>
    </div>
  </div>
  <h1 class="special-font hero-heading absolute bottom-5 right-5 text-black">
    G<b>a</b>ming
  </h1>
</div>
</template>
