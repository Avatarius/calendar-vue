<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import Button from "./Button.vue";
import Dropdown from "./Dropdown.vue";
import { WEEKDAYS } from "../constants/constants";

const { dateString } = defineProps<{ dateString?: string }>();

const date = ref(new Date());

const lang = ref<"ru" | "en">("ru");
const selectedDate = ref<Date | null>(null);

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

function getDate(index: number) {
  const date = new Date(
    year.value,
    month.value,
    index + 1 - firstDayIndex.value
  );
  return date;
}

function getTitle() {
  const month = date.value.toLocaleString(lang.value, { month: "long" });
  const monthString =
    month.charAt(0).toUpperCase() + month.slice(1).toLowerCase();
  return `${monthString} ${year.value}`;
}

function getDayname(index: number) {
  return WEEKDAYS[lang.value][index - 1];
}

function getIsActive(index: number) {
  const today = new Date();
  return (
    today.getDate() === getDate(index).getDate() &&
    today.getMonth() === date.value.getMonth() &&
    today.getFullYear() === date.value.getFullYear()
  );
}

function getIsSelected(index: number) {
  const current = getDate(index);
  return (
    current.getDate() === selectedDate.value?.getDate() &&
    current.getMonth() === selectedDate.value.getMonth() &&
    current.getFullYear() === selectedDate.value.getFullYear()
  );
}

function getIsThisMonth(index: number) {
  const current = getDate(index);
  return current.getMonth() !== month.value;
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
  if (value === "ru" || value === "en") {
    lang.value = value;
  }
}

function handleClick(index: number) {
  const current = getDate(index);
  selectedDate.value = current;
  console.log(current);
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
      <ul class="days">
        <li v-for="value in 7" class="days__item">{{ getDayname(value) }}</li>
      </ul>
      <ul class="list">
        <li
          v-for="value in 35"
          class="list__item"
          :class="{
            list__item_active: getIsActive(value),
            list__item_selected: getIsSelected(value),
            list__item_other: getIsThisMonth(value)
          }"
        >
          <button class="list__button" @click="handleClick(value)">
            {{ getDate(value).getDate() }}
          </button>
        </li>
      </ul>
      <Dropdown
        title="Язык"
        :options="[
          { title: 'Русский', value: 'ru' },
          { title: 'Английский', value: 'en' },
        ]"
        @change="handleLangChange"
      />
  </div>
</template>
<style scoped lang="scss">
@use "../styles/mixins.scss";

.calendar {
  border-radius: 15px;
  box-shadow: 6px 6px 15px rgba(0 0 0 / 0.5);
}

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 22px;
  background-color: rgb(0, 95, 196);
  border-top-left-radius: inherit;
  border-top-right-radius: inherit;
  color: #fff;

  &__title {
    min-inline-size: 250px;
    text-align: center;
    font-size: 33px;
    margin-inline: 40px;
  }

  &__button {
    background-color: red;
  }
}

.list {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: repeat(5, 1fr);
  justify-content: center;

  &__item {
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    inline-size: 100%;
    aspect-ratio: 1;
    &:hover:not(&_selected) {
      background-color: #18aea13e;
    }
    &_active {
      background-color: rgba(0, 95, 196, 0.441);
    }

    &_selected {
      background-color: #9699b5;
    }
    &_other {
      background-color: rgba(220, 220, 220, 0.826);
    }
  }

  &__button {
    @include mixins.button();
    inline-size: 100%;
    block-size: 100%;
  }
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);

  &__item {
    display: flex;
    justify-content: center;
    align-items: center;
    inline-size: 100%;
    aspect-ratio: 1;
    text-transform: uppercase;
    font-size: 12px;
    color: rgb(0, 95, 196);
  }
}
</style>
