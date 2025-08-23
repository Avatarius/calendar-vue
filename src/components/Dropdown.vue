<script setup lang="ts">
import { ref } from "vue";

const { title, options } = defineProps<{
  title: string;
  options: { title: string; value: string }[];
}>();

const emits = defineEmits<{ change: [value: string] }>();

const text = ref(options[0].title);

function handleChange({ title, value }: { title: string; value: string }) {
  emits("change", value);
  text.value = title;
}
</script>
<template>
  <div class="dropdown">
    <h2 class="dropdown__title">{{ title }}</h2>
    <button class="button">{{ text }}</button>
    <ul class="list">
      <li v-for="value in options" class="list__item">
        <button class="list__button" @click="handleChange(value)">
          {{ value.title }}
        </button>
      </li>
    </ul>
  </div>
</template>
<style scoped lang="scss">
@use "../styles/mixins.scss";

.dropdown {
  position: relative;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  background-color: rgb(0, 95, 196);
  border-bottom-left-radius: inherit;
  border-bottom-right-radius: inherit;
  padding: 10px;
  color: #fff;

  &:hover .list {
    display: block;
  }

  &__title {
    margin-inline-end: 10px;
  }
}

.button {
  @include mixins.button();
  inline-size: 100px;
  block-size: 30px;
  background-color: #fff;
  color: #000;
  font-size: 15px;
  border-radius: 5px;
}

.list {
  display: none;
  position: absolute;
  inset-inline-start: 59px;
  inset-block-start: 36px;
  background-color: #fff;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  inline-size: 100px;
  &__item {
    cursor: pointer;
    &:hover {
      background-color: rgba(220, 220, 220, 0.826);
    }
  }
  &__button {
    @include mixins.button();
    inline-size: 100%;
    block-size: 100%;
    padding: 12px;
  }
}
</style>
