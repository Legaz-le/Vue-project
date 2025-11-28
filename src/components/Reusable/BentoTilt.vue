<script setup>
import { ref } from "vue";
const props = defineProps({
  wrapperClass: {
    type: String,
    default: "",
  },
});

const transformStyle = ref("");
const itemRef = ref(null);

const handleMouseMove = (event) => {
  if (!itemRef.value) return;

  const { left, top, width, height } = itemRef.value.getBoundingClientRect();

  const relativeX = (event.clientX - left) / width;
  const relativeY = (event.clientY - top) / height;

  const tiltX = (relativeY - 0.5) * 5;
  const tiltY = (relativeX - 0.5) * -5;

  transformStyle.value = `perspective(700px) rotateX(${tiltX}deg) rotateY(${tiltY}deg) scale3d(.95, .95, .95)`;
};

const handleMouseLeave = () => {
  transformStyle.value = "";
};
</script>

<template>
  <div
    ref="itemRef"
    :class="props.wrapperClass"
    @mousemove="handleMouseMove"
    @mouseleave="handleMouseLeave"
    :style="{ transform: transformStyle }"
  >
    <slot></slot>
  </div>
</template>
