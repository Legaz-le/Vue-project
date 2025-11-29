<script setup>
const props = defineProps({
  src: String,
  clipClass: String
});
</script>

<template>
  <div :class="props.clipClass">
    <img :src="props.src" />
  </div>
</template>