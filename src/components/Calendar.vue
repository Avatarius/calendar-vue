<script setup lang="ts">
import { computed, onMounted, ref } from "vue";

const { dateString } = defineProps<{ dateString?: string }>();

const date = ref(new Date());

/* const year = date.getFullYear();
const month = date.getMonth() ;

console.log(date);


const firstDay = new Date(year, month, 1);
const firstDayIndex = firstDay.getDay();

const lastDay = new Date(year, month + 1, 0);

const lastMonthDate = new Date(year, month, 0); */

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
  const month = date.value.toLocaleString("default", { month: "long" });
  return `${month} ${year.value}`;
}

function getIsActive(index: number) {
  const today = new Date().getDate();
  return today === getDay(index);
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
  <div class="calendar__container">
    <h1 class="calendar__title">{{ getTitle() }}</h1>
    <ul class="calendar__list">
      <li
        v-for="value in 35"
        class="calendar__item"
        :class="{ calendar__item_active: getIsActive(value) }"
      >
        <span class="calendar__day">{{ getDay(value) }}</span>
      </li>
    </ul>
  </div>
</template>
<style scoped lang="scss">
.calendar {
  &__container {
  }
  &__title {
    text-align: center;
    font-size: 25px;
    margin-block-end: 15px;
  }
  &__list {
    display: grid;
    grid-template-columns: repeat(7, 45px);
    grid-template-rows: repeat(5, 45px);
    gap: 15px;
  }

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
  &__day {
  }
}
</style>
