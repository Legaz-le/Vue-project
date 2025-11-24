<script setup>
import { ref, computed } from "vue";
import Button from "./Button.vue";
import { Icon } from "@iconify/vue";

const currentIndex = ref(1);
const hasClicked = ref(false);
const isLoading = ref(true);
const loadedVideo = ref(0);

const totalVideo = 3;
const nextVideo = ref(null);

const handleVideoLoad = () => {
  loadedVideo.value++;
};

const upcomingVideo = computed(() => (currentIndex.value % totalVideo) + 1);

const handleMiniVidClick = () => {
  hasClicked.value = true;

  currentIndex.value = upcomingVideo.value;
};

const videoSrc = (index) => `/videos/hero-${index}.mp4`;
</script>

<template lang="">
  <div class="relative h-dvh w-screen overflow-x-hidden">
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
              ref="nextVideo"
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
          <h1 class="special-font hero-heading text-blue-100">
            redefi<b>n</b>e
          </h1>
          <p class="mb-5 max-w-64 font-robert-regular text-blue-100">
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
                width="20"
                height="24"
              /> </template
          ></Button>
        </div>
      </div>
    </div>
  </div>
</template>
