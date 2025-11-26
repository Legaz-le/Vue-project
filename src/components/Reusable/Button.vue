<template>
  <button
    :id="id"
    class="group relative z-10 w-fit cursor-pointer overflow-hidden rounded-full bg-violet-50 px-7 py-3 text-black "
    :class="containerClass"
  >
    <slot name="leftIcon"></slot>
    <span
      class="relative incline-flex overflow-hidden font-general text-xs uppercase"
      ><div>{{ title }}</div></span
    >
  </button>
</template>

<script>
export default {
  props: ["id", "title", "containerClass"],
};
</script>
