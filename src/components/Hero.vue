<script setup>
import { ref } from "vue";

const currentIndex = ref(1);
const hasClicked = ref(false);
const isLoading = ref(true);
const loadedVideo = ref(0);

const totalVideo = 4;
const nexVideo = ref(null);

const handleVideoLoad = () => {
  loadedVideo.value++
};

function handleMiniVidClick () {
  hasClicked.value = true;

  currentIndex.value = (currentIndex.value % totalVideo) + 1;
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
              :src="videoSrc(currentIndex)"
              ref="nexVideo"
              loop
              muted
              id="current-video"
              class="size-64 origin-center scale-150 object-cover object-center"
              @loadeddata="handleVideoLoad"
            ></video>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
