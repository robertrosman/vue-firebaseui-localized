<script setup>
import SkeletonButton from "./SkeletonButton.vue";
import { defineProps } from "vue";
const props = defineProps({
  buttonAmount: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div class="skeleton-container">
    <SkeletonButton v-for="index in props.buttonAmount" />
  </div>
</template>

<style scoped>
.skeleton-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
}
</style>
