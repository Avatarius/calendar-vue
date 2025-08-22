<script setup lang="ts">
const { side } = defineProps<{ side: "left" | "right", onClick?: () => void }>();

defineEmits<{click: () => void}>();
</script>
<template>
  <button class="button" @click="$emit('click')">
    <img src="../../public/left-arrow.svg" v-if="side === 'left'" alt="left arrow" class="button__img"/>
    <img src="../../public/right-arrow.svg" v-else alt="right arrow" class="button__img"/>
  </button>
</template>
<style scoped lang="scss">
.button {
  cursor: pointer;
  inline-size: 25px;
  block-size: 25px;
  padding: 0;
  margin: 0;
  appearance: none;
  border: none;
  background-color: transparent;
  font-family: inherit;
  transition: scale 0.15s linear;
  &:hover {
    scale: 1.15;
  }
  &__img {
    display: block;
    inline-size: 100%;
    block-size: 100%;
  }
}
</style>
