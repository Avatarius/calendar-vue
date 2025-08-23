<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import Button from "./Button.vue";
import Dropdown from "./Dropdown.vue";

const { dateString } = defineProps<{ dateString?: string }>();

const date = ref(new Date());
const lang = ref<'ru' | 'en'>('ru');

const year = computed(() => {
  return date.value.getFullYear();
});

const month = computed(() => {
  return date.value.getMonth();
});

const firstDay = computed(() => {
  return new Date(year.value, month.value, 1);
});

const firstDayIndex = computed(() => {
  return firstDay.value.getDay();
});

function getDay(index: number) {
  const current = new Date(
    year.value,
    month.value,
    index + 1 - firstDayIndex.value
  );
  return current.getDate();
}

function getTitle() {
  const month = date.value.toLocaleString(lang.value, { month: "long" });
  return `${month} ${year.value}`;
}

function getIsActive(index: number) {
  const today = new Date();
  return (
    today.getDate() === getDay(index) &&
    today.getMonth() === date.value.getMonth() &&
    today.getFullYear() === date.value.getFullYear()
  );
}

function increment() {
  date.value = new Date(date.value);
  date.value.setMonth(date.value.getMonth() + 1);
}

function decrement() {
  date.value = new Date(date.value);
  date.value.setMonth(date.value.getMonth() - 1);
}

function handleLangChange(value: string) {
  if (value === 'ru' || value === 'en') {
    lang.value = value;
  }
}

onMounted(() => {
  if (!dateString) return;
  const dateFromStr = new Date(dateString);
  const isValid = !isNaN(dateFromStr.getTime());
  if (isValid) {
    date.value = dateFromStr;
  }
});
</script>
<template>
  <div class="calendar">
    <header class="header">
      <Button side="left" @click="decrement" />
      <h1 class="header__title">{{ getTitle() }}</h1>
      <Button side="right" @click="increment" />
    </header>
    <ul class="list">
      <li
        v-for="value in 35"
        class="list__item"
        :class="{ list__item_active: getIsActive(value) }"
      >
        <span>{{ getDay(value) }}</span>
      </li>
    </ul>
    <Dropdown title="Язык" :options="[{title: 'Русский', value: 'ru'}, {title: 'Английский', value: 'en'}]" @change="handleLangChange"/>
  </div>
</template>
<style scoped lang="scss">
.calendar {
  border: 1px solid black;
  border-radius: 15px;
  padding: 15px;
}

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  margin-block-end: 20px;

  &__title {
    min-inline-size: 150px;
    text-align: center;
    font-size: 25px;
    margin-inline: 40px;
  }

  &__button {
    background-color: red;
  }
}

.list {
  display: grid;
  grid-template-columns: repeat(7, 45px);
  grid-template-rows: repeat(5, 45px);
  gap: 15px;

  &__item {
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    inline-size: 100%;
    aspect-ratio: 1;
    border: 1px solid cyan;

    &_active {
      background-color: red;
    }
  }
}
</style>
