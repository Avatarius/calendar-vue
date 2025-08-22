<script setup lang="ts">
import Calendar from './components/Calendar.vue';

</script>

<template>
<div class="container">
  <Calendar date-string="2025-08-21"/>
</div>
</template>

<style scoped>
.container {
  min-block-size: 100dvh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
